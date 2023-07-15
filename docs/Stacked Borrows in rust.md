---
description:
aliases: 
tags: 
created: 2023-04-07T21:39:31
updated: 2023-07-15T21:33:03
title: Stacked Borrows in rust
---
- https://rust-unofficial.github.io/too-many-lists/fifth-stacked-borrows.html
- unsafe 코드를 작성하고나서부터 상황이 꼬이기 시작했다. 특히 [[MIRI - Mid-level Intermediate Representaion Interpreter]]를 통한 UB 체크가 실패하고나서부터!
- Safe 코드에서 borrow checker는 컴파일 시간에 Stacked Borrow를 체크할 수 있다. 하지만 Unsafe 코드에서는 동작하지 않는다. 따라서 런타임에 멀쩡해 보이는 코드일지라도 MIRI를 사용하여 내가 작성한 코드에 어떤 취약점이 있는지 파악하는 것을 습관화 해야 한다.
- 그래서, Stacked Borrow가 무엇이냐 하면, 가끔 mutable reference aliasing을 시도할 때 볼 수 있는 컴파일 에러가 바로 그 설명을 한다.

다음 코드는 컴파일 에러를 일으킨다.

```rust
fn main() {
    let mut data = 10;
    let ref1 = &mut data;
    let ref2 = &mut *ref1; // mutable reference aliasing happen! (reborrow)

    *ref1 += 1;
    *ref2 += 2;

    assert_eq!(13, data);
}
```

무엇이 문제인가 보면, `ref2`가 레퍼런스를 아직 반납하지 않았는데 `ref1`이 갑자기 자기 것인 냥 사용하고 있다는 것이었다.

```
error[E0503]: cannot use `*ref1` because it was mutably borrowed
 --> src/main.rs:6:5
  |
4 |     let ref2 = &mut *ref1; // mutable reference aliasing happen!
  |                ---------- borrow of `*ref1` occurs here
5 |
6 |     *ref1 += 1;
  |     ^^^^^^^^^^ use of borrowed `*ref1`
7 |     *ref2 += 2;
  |     ---------- borrow later used here
```

borrow checker는 여러 reborrowed reference들이 stack의 속성같이 alias 되도록 강제한다. 새로운 alias를 정의한다는 것은 마치 스택에서의 push와 같은 연산을 수행하고, 명시적으로 드랍하지는 않아도 마지막으로 그 alias를 사용한 직후 pop과 같은 연산을 수행한다. 위의 코드를 개념적으로 본다면 다음과 같을 수 있다. 각 블록 안에서는 해당 alias만 사용할 수 있다.

```rust
let mut data = 10; {
	let ref1 = &mut data; { // ref1 borrows data by mutable
		let ref2 = &mut *ref1 { // ref2 reborrows ref1's pointee by mutable
		}
	}
}
```

따라서, 이 컨셉에 따르면 `ref2` 블록 안에서 `ref1`을 사용하고 있기 때문에 마치 스택에 최상단 원소를 제거하지도 않은 채 중간 원소를 사용하는 것으로 바라볼 수 있고, 이는 곧 Stacked Borrow 규칙을 위반한 것이 된다.

> Whatever's at the top of the borrow stack is "live" and knows it's effectively unaliased.  
> 빌린 스택의 꼭대기에 있는 것은 무엇이든지 간에 살아있고, 또한 효율적으로 별칭이 없다는 것을 알고 있습니다. (보장합니다)

# Stacked Borrow가 뭔지는 이제 알겠어, 근데 왜 필요한 건데?

프로그래머로 하여금 내가 소유하고 있는 레퍼런스에 대한 참조투명성을 보장하는 아주 훌륭한 지표가 되기 때문이다. C world에서의 포인터는 단순 숫자이다. 따라서 어떤 변수나 객체에 대한 소유권또한 존재하지 않고 단지 내가 어떤 변수에 대한 포인터를 가지고 있는 동안 만큼은 다른 누군가가 같은 주소에 장난질을(값을 변경한다거나, delete를 수행한다거나) 하지 않기를 바랄 뿐이다.

러스트에서 소유권 개념을 적용하면서부터 내가 어떤 변수에 대한 소유권을 가지고 있는 동안 다른 존재가 함부로 그 공간을 훼방 놓을 수 없게 되었다. borrowing도 마찬가지이다. [[Permission Handling]]을 공부하면서 느낀 희열의 원천이 바로 이곳인 것이다. 

서슬퍼런 눈을 뜬 borrow checker의 감시를 피해 달아난 unsafe realm에서도 MIRI의 칼날을 피해갈 순 없었다. 

# 다음 원칙들만 기억하자.

1.  At the start of a method, use the input references to get our raw pointers
2.  Do our best to only use unsafe pointers from this point on
3.  Convert our raw pointers back to safe pointers at the end if needed

다양한 실험 실습은 [다음 링크](https://rust-unofficial.github.io/too-many-lists/fifth-testing-stacked-borrows.html) 에서 확인바람.

# Shared references cannot be mutated even aliased

한 번 불변으로 변수를 빌렸다면 하위 모든 reborrowed references들 또한 불변이어야 한다. (당연한거 아님?) unsafe pointer들은 그딴거 깡끄리 무시하고 아래처럼 `as *const i32 as *mut i32` 로 다시 가변 원시 포인터로 변환할 수 있는데, 이런 짓 하지 말란거임...

```rust
fn main() {
    fn opaque_read(val: &i32) {
        println!("{}", val);
    }
    unsafe {
        let mut data = 10;
        let mref1 = &mut data;
        let ptr2 = mref1 as *mut i32;
        let sref3 = &*mref1; // reborrow as immutable
        let ptr4 = sref3 as *const i32 as *mut i32; // very very unsafe

        *ptr4 += 4; // Attempting a write access using <2787> at alloc 1461[0x0], but that tag only grants SharedReadOnly permission for this location
        opaque_read(&*ptr4);
        opaque_read(sref3);
        *ptr2 += 2;
        *mref1 += 1;

        opaque_read(&data);
    }
}
```

# Safe pointers are tagged, unsafe ones aren't

아래 코드는 문제를 낳지 않는다. `ptr2` 부터 `ptr5`까지 전부 `*mut i32` 타입인데, 레퍼런스 `ref1`로부터 내려받은 원시 포인터끼리는 빌림규칙이 적용되지 않는다. 따라서 순서를 뒤섞어 놓는다는 일로 MIRI가 불평을 늘어놓지 않는다는 것이다.

```rust
fn opaque_read(val: &i32) {
	println!("{}", val);
}
unsafe {
	let mut data = 0;
	let ref1 = &mut data;
	let ptr2 = ref1 as *mut i32;
	let ptr3 = ptr2;
	let ptr4 = ptr3;
	let ptr5 = ptr2;

	*ptr2 += 1;
	*ptr5 += 1;
	*ptr4 += 1;
	*ptr3 += 1;
	*ptr4 += 1;

	opaque_read(&data);
}
```

# Interior mutability with `UnsafeCell`

`UnsafeCell`은 우리에게 단순 원시 포인터를 넘겨주는 `get()` 메서드를 가지고 있다. 이는 빌림규칙도 따르지 않고 `Cell` 처럼 변수나 레퍼런스가 불변이어도 내부는 항상 가변이라는 점.
