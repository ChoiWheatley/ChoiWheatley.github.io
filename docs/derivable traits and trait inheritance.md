---
description:
aliases: 
tags: 
created: 2023-04-01T11:03:08
updated: 2023-07-11T15:21:09
title: derivable traits and trait inheritance
---
- [Rust book - Appendix C: Derivable Traits](https://doc.rust-lang.org/book/appendix-03-derivable-traits.html)
- [questions about deriving traits in stack overflow](https://stackoverflow.com/a/50040689/21369350)
- [뭐라고? trait가 trait를 상속할 수 있다고? stack overflow](https://stackoverflow.com/a/47966422/21369350)
```rust
trait A {}
trait B: A {}
struct S;
impl B for S {}
impl A for S {} // separately implement both B and A

let x: &B = &S;
```
- [Rust is not an OOP, which cannot upcast to base type](https://stackoverflow.com/questions/28632968/why-doesnt-rust-support-trait-object-upcasting)
- 