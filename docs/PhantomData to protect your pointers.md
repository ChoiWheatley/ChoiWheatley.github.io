---
description:
aliases: 
tags: 
created: 2023-04-09T22:20:22
updated: 2023-07-15T21:33:03
title: PhantomData to protect your pointers
---
- https://rust-unofficial.github.io/too-many-lists/sixth-variance.html
- 아.................. 이건 또 뭔데, 빙한테 물어보자...  
PhantomData is a Rust feature that is used when working with unsafe code. It is used when types or lifetimes are logically associated with a struct, but not actually part of a field. This most commonly occurs with lifetimes. For instance, the Iter for `&'a [T]` is (approximately) defined as follows:

```rust
struct Iter<'a, T: 'a> {
    ptr: *const T,
    end: *const T,
    _marker: PhantomData<&'a T>,
}
```

Here, `_marker` is a field of type `PhantomData<&'a T>`. The purpose of this field is to indicate that this struct has an associated lifetime `'a`, even though it doesn’t actually contain any references with that lifetime.

Another important example is Vec, which is (approximately) defined as follows:

```rust
#![allow(unused)]
fn main() {
    struct Vec<T> {
        data: *const T,
        // *const for variance!
        len: usize,
        cap: usize,
        _marker: PhantomData<&'a [T]>,
    }
}
```

Here, `_marker` is a field of type `PhantomData<&'a [T]>`. [The purpose of this field is to indicate that this struct has an associated lifetime `'a`, even though it doesn’t actually contain any references with that lifetime.](https://doc.rust-lang.org/nomicon/phantom-data.html)[1](https://doc.rust-lang.org/nomicon/phantom-data.html)[2](https://doc.rust-lang.org/beta/core/marker/struct.PhantomData.html)

I hope this helps! Let me know if you have any more questions.
