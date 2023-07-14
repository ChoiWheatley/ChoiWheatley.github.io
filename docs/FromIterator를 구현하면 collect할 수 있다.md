---
description:
aliases: 
tags: 
created: 2023-04-10T23:19:44
updated: 2023-07-11T15:21:08
title: FromIterator를 구현하면 collect할 수 있다
---
- https://doc.rust-lang.org/std/iter/trait.FromIterator.html
- 냠냠
```rust
pub trait FromIterator<A>: Sized {
	fn from_iter<T>(iter: T) -> Self
	where T: IntoIterator<Item = A>;
}
```
- 함수 시그니처를 보면 알 수 있듯이, 어떤 Iterable한 인자가 들어오면 해당 타입으로 변환을 할 수 있다. 
> If you want to create a collection from the contents of an iterator, the [`Iterator::collect()`](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.collect "Iterator::collect()") method is preferred
> 만약 당신이 이터레이터로부터 컬렉션을 만들고 싶다면 `collect` 메서드를 사용하는 것을 추천함.

- What on earth is `collect`???
- https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.collect
- 