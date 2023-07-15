---
description:
aliases: 
tags: 
created: 2023-03-08T23:43:41
updated: 2023-07-15T21:33:04
title: Mutable reference is a non Copy type
---
https://cotigao.medium.com/mutable-reference-in-rust-995320366e22

## mutable reference cannot outlive

다음 코드의 scope가 벗어나게되면 x가 해제되고 y는 dangling pointer가 되게 된다. 따라서 컴파일 에러가 발생한다.

```rust
let mut i = 1;
let j = {
	let x = &mut i;
	let y = &x;
	&**y
};
```

## immutable reference COPIED out

단지 불변 레퍼런스로 바꿨을 뿐인데 컴파일이 잘 된다. 이유로는 x가 단순히 y에 복사되어 들어가기 때문에 x가 소멸되어도 y는 i를 제대로 가리킬 수 있는 것이다.

```rust
let mut i = 1;
let j = {
	let x = &i;
	let y = &x;
	&**y
};
```
