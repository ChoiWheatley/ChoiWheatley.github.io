---
aliases: 
tags: 
description:
title: pointer array and array pointer
created: 2023-09-01T11:30:13
updated: 2023-09-01T11:46:10
---
- [[0017 C 🍎]]
- [gfg](https://www.geeksforgeeks.org/pointer-array-array-pointer/)
___

> what do you mean by `int (*ptr)[10]`

A) ptr is an array of pointers to 10 integers

B) ptr is a pointer to an array of 10 integers

C) ptr is an array of 10 integers

D) Invalid statement

___

정답은 D인 줄 알았으나 B였다. 꼭 변수명 앞에 `*`을 붙이고 괄호로 감싼다고 그것이 함수포인터라는 것은 아니다. 처음엔 함수포인터의 형태 `type (*var_name)(args)`가 아니라고 판단하여 D를 찍은 것이었고, 사실 이것은 **포인터 배열**과 **배열 포인터** 사이의 관계에 대한 내용을 이해하고 있는지에 대한 문제였던 것이다. 

`int *ptr[10]` => ptr는 10개의 포인터를 가리키고 있는 포인터이다.

![[Pasted image 20230901114608.png]]

`int (*ptr)[10]` => 포인터인 ptr은 10개의 int형 원소를 참조하고 있다.

![[Pasted image 20230901113630.png]]

`int ptr[10]` 또한 10개의 원소를 참조하고 있는 포인터 ptr을 선언한다. 따라서 `int (*ptr)[10]`과 완전히 동일한 표현이다.

![[Pasted image 20230901113630.png]]
