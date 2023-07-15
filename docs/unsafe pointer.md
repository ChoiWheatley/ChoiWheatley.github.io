---
description:
aliases: 
tags: 
created: 2023-03-23T21:27:06
updated: 2023-07-15T21:33:03
title: unsafe pointer
---
- https://doc.rust-lang.org/book/ch19-01-unsafe-rust.html#dereferencing-a-raw-pointer
- https://cheats.rs/#references-pointers-ui

> Creating a pointer does no harm; it's only when we try to access the value that is points at that we might end up dealing with an invalid value.  
> 포인터를 만드는 것 자체는 문제가 없습니다. 다만 포인터로부터 데이터를 접근하려는 것은 유효하지 않은 값을 참조할 수 있으므로 위험합니다.

```rust
let mut num = 5;

let r1 = &num as *const i32;
let r2 = &mut num as *mut i32;

unsafe {
	println!("r1 is : {}", *r1);
	println!("r2 is : {}", *r2);
}
```
