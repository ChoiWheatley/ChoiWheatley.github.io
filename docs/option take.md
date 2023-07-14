---
description:
aliases: 
tags: 
created: 2023-03-31T20:32:51
updated: 2023-07-11T15:21:07
title: option take
---
- https://doc.rust-lang.org/std/option/enum.Option.html#method.take
- Takes the value out of the option, leaving a `None` in its place.

signiture
```rust
pub fn take(&mut self) -> Option<T>
```

example
```rust
let mut x = Some(2);
let y = x.take();
assert_eq!(x, None);
assert_eq!(y, Some(2));

let mut x: Option<u32> = None;
let y = x.take();
assert_eq!(x, None);
assert_eq!(y, None);

```