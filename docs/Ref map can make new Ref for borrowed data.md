---
description:
aliases: 
tags: 
created: 2023-04-04T23:42:39
updated: 2023-07-11T15:21:07
title: Ref map can make new Ref for borrowed data
---
- https://doc.rust-lang.org/std/cell/struct.Ref.html#method.map
- `Ref<TooComplexType>` 를 `Ref<SimpleType>` 으로 쪼갤때 사용된다.

```rust
use std::cell::{RefCell, Ref};

let c = RefCell::new(vec![1, 2, 3]);
let b1: Ref<Vec<u32>> = c.borrow();
let b2: Result<Ref<u32>, _> = Ref::filter_map(b1, |v| v.get(1));
assert_eq!(*b2.unwrap(), 2);

```