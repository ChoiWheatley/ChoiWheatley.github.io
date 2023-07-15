---
description:
aliases: 
tags: 
created: 2023-03-31T20:58:47
updated: 2023-07-15T21:33:03
title: option map
---
- https://doc.rust-lang.org/std/option/enum.Option.html#method.map
- Maps an `Option<T>` to `Option<U>` by applying a function to a contained value
- 기존 `Option`을 consume한다는거에 유의. 만약 옵션 타입 내부에 있는 값을 move하지 않으면서 빌려오고 싶다면 [[option as_ref]] 를 사용하면 된다.

signature

```rust
pub fn map<U, F>(self, f: F) -> Option<U>
```

example  
Calculates the length of an `Option<String>` as an `Option<usize>`, consuming the original:

```rust
let maybe_some_string = Some(String::from("Hello, World!"));
// `Option::map` takes self *by value*, consuming `maybe_some_string`
let maybe_some_len = maybe_some_string.map(|s| s.len());

assert_eq!(maybe_some_len, Some(13));
```
