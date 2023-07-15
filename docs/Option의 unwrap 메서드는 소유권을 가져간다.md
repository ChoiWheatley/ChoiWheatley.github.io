---
description:
aliases: 
tags: 
created: 2023-03-17T16:46:11
updated: 2023-07-15T21:33:03
title: Option의 unwrap 메서드는 소유권을 가져간다
---
[unwrap](https://doc.rust-lang.org/std/option/enum.Option.html#method.unwrap) 메서드의 함수 시그니처로 `&self` 가 아닌, `self`를 받는다. 그래서 다음 코드는 컴파일 에러를 발생시킨다.

```rust
fn get_or_default(arg: &Option<String>) -> String {
	if arg.is_none() {
		return String::new();
	}
	let s = arg.unwrap();
	s.clone()
}
```

`arg: Option<String>`를 빌려온 주제에 `unwrap`으로 String을 Consume 하려고 한다.즉, arg를 훔친것도 모자라 내부상태마저도 바꾸려는 짓을 하는 것이다. 다음은 에러메시지이다.

```
error[E0507]: cannot move out of `*arg` which is behind a shared reference
   --> test.rs:7:13
    |
7   |     let s = arg.unwrap();
    |             ^^^^--------
    |             |   |
    |             |   `*arg` moved due to this method call
    |             help: consider calling `.as_ref()` or `.as_mut()` to borrow the type's contents
    |             move occurs because `*arg` has type `Option<String>`, which does not implement the `Copy` trait
```

그렇다면 Option타입이 들고있는 값을 조심스럽게 빌릴 수는 없을까? note 에서 보여주다시피 `.as_ref()` 또는 `.as_mut()`를 사용하면 된다. `Option<T>` 를 `Option<&T>` 로 바꿔주기 때문에 unwrap을 하거나 map 등의 작업을 해도 consume 하지 않는다.
