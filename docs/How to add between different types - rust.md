---
description:
aliases: 
tags: 
created: 2023-04-04T15:03:38
updated: 2023-07-15T21:33:04
title: How to add between different types - rust
---
- https://users.rust-lang.org/t/how-to-add-different-types/79178

```rust
impl std::ops::Add<B> for A {
    type Output = B;
    fn add(self, rhs: B) -> Self::Output { todo!() }
}
impl std::ops::Add<A> for B {
    type Output = B;
    fn add(self, rhs: A) -> Self::Output { todo!() }
}
```
