---
description:
aliases: 
tags: 
created: 2023-04-06T23:03:21
updated: 2023-07-11T15:21:08
title: enumerate and zip
---
- [iterator::enumerate](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.enumerate)
- [iterator::zip](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.zip)
- `enumerate`는 이터레이션을 진행할 때 자동으로 카운트를 세 주는 간편한 녀석이다.
```rust
let a = ['a', 'b', 'c'];
let iter = a.iter().enumerate();
assert_eq!(iter.next(), Some((0usize, &'a')));
assert_eq!(iter.next(), Some((1usize, &'b')));
assert_eq!(iter.next(), Some((2usize, &'c')));
assert_eq!(iter.next(), None);
```
- 하지만 튜플의 첫번째 원소는 항상 `usize` 고정이고, 뭐 나중에 캐스팅 해도 되긴 하지만 굳이 다른 타입으로 바꿔쓰고 싶다면 `zip` 을 사용하라고 한다. 그런데 약간의 실수를 저질렀다. `a.zip(b)` 이므로 `(a의 원소, b의 원소)` 순이 당연한 걸 반대로 적어놓고 있었던 것이다!
```rust
let a = ['a', 'b', 'c'];
let iter = a.iter().zip(0i32..); // lazy evaluation의 위대함
assert_eq!(iter.next(), Some((&'a', 0i32))); // 순서 유의!!
assert_eq!(iter.next(), Some((&'b', 1i32)));
assert_eq!(iter.next(), Some((&'c', 2i32)));
assert_eq!(iter.next(), None);
```