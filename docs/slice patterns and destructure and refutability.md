---
description:
aliases: 
tags: 
created: 2023-04-07T02:03:31
updated: 2023-07-15T21:33:03
title: slice patterns and destructure and refutability
---
- https://blog.rust-lang.org/inside-rust/2020/03/04/recent-future-pattern-matching-improvements.html#subslice-patterns-head-tail--
- [refutablity - the book](https://doc.rust-lang.org/book/ch18-02-refutability.html)

1.26버전에 도입된 [fixed length slice pattern](https://blog.rust-lang.org/2018/05/10/Rust-1.26.html#basic-slice-patterns) 덕분에 아래와 같이 배열도 destructure가 가능해졌다. 작은 문제가 하나 있다면 `my_array`의 사이즈와 destructured 된 배열의 사이즈가 반드시 일치해야만 한다는 것이다. 게다가 오직 최대 3개의 임의의 배열에 한해서만 destructure가 가능하니 이 얼마나 분개할 일인가!

```rust
let my_array = [1, 2, 3];
let [a, b, c] = my_array;
```

1.42버전부터 `..` 서브슬라이스 패턴을 사용하여 가변길이의 패턴도 받을 수 있게 되었다.

```rust
let my_array = [1, 2, 3, 4, 5];
let [a, b, c, ..] = my_array;
```

처음과 끝 원소만 분해하고 싶다고? 가능합니다

```rust
let my_array = [1, 2, 3, 4, 5];
let [a, .., b] = my_array;
assert_eq!(a, 1);
assert_eq!(b, 5);
```
