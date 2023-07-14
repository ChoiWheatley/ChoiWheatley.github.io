---
description:
aliases: 
tags: 
created: 2023-03-31T18:46:30
updated: 2023-07-11T15:21:09
title: core mem replace
---
- https://doc.rust-lang.org/nightly/core/mem/fn.replace.html
- Moves `src` into the referenced `dest`, returning the previous `dest` value.
- If you want to replace the values of two variables, see [[core mem swap]]
- If you want to replace with a default value, see [[core mem take]] .
- `replace` allows consumption of a struct field by replacing it with another value.

내부 구현을 보자
```rust
pub const fn replace<T>(dest: &mut T, src: T) -> T {
    // SAFETY: We read from `dest` but directly write `src` into it afterwards,
    // such that the old value is not duplicated. Nothing is dropped and
    // nothing here can panic.
    unsafe {
        let result = ptr::read(dest);
        ptr::write(dest, src);
        result
    }
}
```

`replace`를 사용하면 누가봐도 당연하지만 컴파일러가 싫어하는 코드를 안전하게 작성할 수 있다.
```rust
use std::mem;

struct Buffer<T>(Vec<T>);

impl<T> Buffer<T> {
	/// This is not ok,
	/// error: cannot move out of dereference of `&mut` pointer
	fn replace_index(&mut self, i: usize, v: T) -> T {
		let t = self.0[i];
		self.0[i] = v;
	}
	/// This is Ok
	fn replace_index(&mut self, i: usize, v: T) -> T {
		mem::replace(&mut self.0[i], v)
	}
}
```
