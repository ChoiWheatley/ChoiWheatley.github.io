---
description:
aliases: 
tags: 
created: 2023-03-31T20:35:57
updated: 2023-07-15T21:33:03
title: option replace
---
- https://doc.rust-lang.org/std/option/enum.Option.html#method.replace
- Replaces the actual value in the option by the value given in parameter, returning the old value if present, leaving a `Some` in its place without deinitializing either one.

```rust
pub fn replace(&mut self, value: T) -> Option<T>
```
