---
description:
aliases: 
tags: programming/rust 
created: 2023-03-08T15:05:42
updated: 2023-07-15T21:33:03
title: Permission Handling
---
https://rust-book.cs.brown.edu/ch04-02-references-and-borrowing.html
- All variables can read, own, and (optionally) write their data.
	- 모든 변수는 읽기, 쓰기(선택적), 소유권한을 가지고 있다.
- Creating a reference will transfer permissions from the borrowed path to the reference.
	- 변수에 대한 레퍼런스를 생성하는 순간 레퍼런스에게 자신의 권한을 넘겨준다.
	- 레퍼런스가 불변이라면, 변수의 권한은 읽기로 축소된다.
	- 레퍼런스가 가변이라면 변수의 권한은 아무것도 없다.
- Permissions are returned once the reference's lifetime has ended.
	- 레퍼런스의 라이프타임이 종료되면 변수에게 권한이 다시 돌아온다.
- Data must outlive all references that point to it.
	- 값은 레퍼런스보다 생존기간이 길다.

borrowing은 얼핏보면 c/cpp의 포인터와 개념이 비슷하지만 사실은 권한위임과 권한 되돌려받기의 과정의 추상화인 것이다. borrowing이 없다면 우리는 매번 ownership을 넘겨주고 스코프에서의 사용이 끝나면 다시 ownership을 넘겨받아야만 했을 것이다.
