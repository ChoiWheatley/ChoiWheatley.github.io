---
description:
aliases: 
tags: 
created: 2023-04-07T13:39:56
updated: 2023-07-15T21:33:03
title: Using the Newtype Pattern to Implement External Traits on External Types
---
- [the book](https://doc.rust-lang.org/book/ch19-03-advanced-traits.html#using-the-newtype-pattern-to-implement-external-traits-on-external-types)
- [E0117](https://doc.rust-lang.org/stable/error_codes/E0117.html)
- [RFC 1023](https://github.com/rust-lang/rfcs/blob/master/text/1023-rebalancing-coherence.md)  orphan rules에 대한 설명, we limit the use of _negative reasoning_ to obey the orphan rules. That is, just as a crate cannot define an impl `Type: Trait` unless `Type` or `Trait` is local, it cannot rely that `Type: !Trait` holds unless `Type` or `Trait` is local.
- [Reason of Why Restriction Is Valid](https://gist.github.com/nikomatsakis/bbe6821b9e79dd3eb477)
- 러스트의 확장성에는 제한이 걸려있다. 적어도 트레잇 또는 타입이 현재 크레이트에 있어야만 한다. 그렇지 않은 경우에는 "NewType" 패턴을 사용하여 현재 스코프에 간단한 래퍼 타입을 만드는 것으로 문제 해결이 가능하고, 심지어 zero-cost-abstraction이라서 부담없이 사용할 수 있다.

다음 코드는 컴파일 에러가 발생한다.

```rust
impl Ord for [i32; 2] {
	fn cmp(&self, other: &Self) -> Ordering {
		if self[0] == other[0] {
			self[1].cmp(&other[1])
		} else {
			self[0].cmp(&other[0])
		}
	}
}
```

컴파일러가 타입 `[i32; 2]`이 foreign이고, 동시에 `Ord` 트레잇 또한 현재 크레이트에 정의되어 있지 않다고 불평을 내고있다.

```
error[E0117]: only traits defined in the current crate can be implemented for arbitrary types
 --> src/main.rs:4:5
  |
4 |     impl Ord for [i32; 2] {
  |     ^^^^^^^^^^^^^--------
  |     |            |
  |     |            this is not defined in the current crate because arrays are always foreign
  |     impl doesn't use only types from inside the current crate
  |
  = note: define and implement a trait or new type instead

For more information about this error, try `rustc --explain E0117`.
```

해결방법은 다양하다. 하나씩 핥아보고 가자.

# Newtype Pattern Inspired by Haskell programming language

래퍼타입을 사용하여 foreign type을 현재 크레이트의 타입인냥 사용할 수 있다.

```rust
use core::cmp::*;
#[derive(Eq, PartialEq, PartialOrd)]
struct Wrapper([i32; 2]);

impl Ord for Wrapper {
    fn cmp(&self, other: &Self) -> Ordering {
        if self.0[0] == other.0[0] {
            self.0[1].cmp(&other.0[1])
        } else {
            self.0[0].cmp(&other.0[0])
        }
    }
}
fn main() {
    let lhs = [1, 2];
    let rhs = [3, 4];
    assert!(Wrapper(lhs) < Wrapper(rhs));
    assert!(lhs < rhs); // 기본적으로 배열끼린 비교가 가능했구나
}
```

# Ensure at least one local type is referenced by the `impl`

래퍼타입을 쓰기 싫다면 `From` 트레잇을 구현한 임의의 타입으로 대치할 수 있다. (효용성은 잘 모르겠음)

```rust
pub struct Foo; // you define your type in your crate

impl Drop for Foo { // and you can implement the trait on it!
    // code of trait implementation here
}

impl From<Foo> for i32 { // or you use a type from your crate as
                         // a type parameter
    fn from(i: Foo) -> i32 {
        0
    }
}
```

# Define a trait locally and implement that instead

타입을 로컬로 만드는 것이 아닌, 트레잇을 로컬로 만드는 전략이다.

```rust
trait Bar {
    fn get(&self) -> usize;
}

impl Bar for u32 {
    fn get(&self) -> usize { 0 }
}
```
