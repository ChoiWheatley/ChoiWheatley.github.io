---
description:
aliases: 
tags: 
created: 2023-03-08T22:14:56
updated: 2023-07-11T15:21:08
title: Indexing is opaque
---
https://www.reddit.com/r/rust/comments/ddd7qm/comment/f2ftp47/?utm_source=share&utm_medium=web2x&context=3

> Indexing is an arbitrary computation, which means it's opaque to the compiler and thus borrows the **entire** structure to perform the operation.

다음 코드는 컴파일이 안된다.

```rust
let v: Vec<String> = vec![
	String::from("Hello"), 
	String::from("World")
];

let mut s_ref = &mut v[0];
let mut s_ref1 = &mut v[1]; // ERROR cannot borrow `v` as mutable more than once at a time
```

`v[0]` 라고 해서 v의 첫번째 원소에 대한 레퍼런스만을 빌려왔겠다고 생각하기 쉽지만, 러스트 컴파일러는 그 내부에 대해 잘 모르기 때문에 `v` 를 통째로 빌렸다고 생각하게 된다. 따라서 두번째 borrowing에서 컴파일러는 "엥? v를 또 가변레퍼런스로 빌려가게?" 하면서 오류를 뱉어내게 되는 것이다.

### 해결방법

1. perform your operations sequentially
2. [`split_at_mut`](https://doc.rust-lang.org/std/primitive.slice.html#method.split_at_mut) 함수 사용하여 소유권을 따로 빌리기
3. [`iter_mut`](https://doc.rust-lang.org/std/primitive.slice.html#method.iter_mut) 함수를 사용하여 각 원소에 대한 소유권을 빌려오기