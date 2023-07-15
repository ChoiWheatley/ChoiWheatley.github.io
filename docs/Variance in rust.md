---
description:
aliases: 
tags: 
created: 2023-04-09T21:35:52
updated: 2023-07-15T21:33:03
title: Variance in rust
---
- [TMLL](https://rust-unofficial.github.io/too-many-lists/sixth-variance.html) 에서 타입 가변성과 공변성, 불변성에 대한 토막에 관한 이야기가 나와서 스크랩 한다.
- 기본적으로 불변타입이 아닌 대부분의 타입들은 공변이라고 한다. 불변타입먼저 알아보자.
- 불변타입
	- `&mut T`
	- `Cell<T>` 사실 얘는 interior mutability 때문에 `&mut T` 라고 봐야 하기 때문이다.
	- `*mut T` 얘도 언제든 `&mut T`로 변환할 수 있기 때문!
- 가변타입 (공변이 가능한)
	- `T` 값이 변하지 않는다면 기본적으로 타입 가변이다.
	- Collections such as `Vec<T>` 의미론적으로 T잖아. 따라서 가변.
