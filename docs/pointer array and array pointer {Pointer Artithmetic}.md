---
aliases: 
tags: 
description:
title: pointer array and array pointer {Pointer Artithmetic}
created: 2023-09-01T11:30:13
updated: 2023-09-01T14:36:48
---
- [[0017 C 🍎]]
- [gfg](https://www.geeksforgeeks.org/pointer-array-array-pointer/)
- [How come an array's address is equal to its value in C? {SO}](https://stackoverflow.com/questions/2528318/how-come-an-arrays-address-is-equal-to-its-value-in-c)
___

> what do you mean by `int (*ptr)[10]`

A) ptr is an array of pointers to 10 integers

B) ptr is a pointer to an array of 10 integers

C) ptr is an array of 10 integers

D) Invalid statement

___

정답은 D인 줄 알았으나 B였다. 꼭 변수명 앞에 `*`을 붙이고 괄호로 감싼다고 그것이 함수포인터라는 것은 아니다. 처음엔 함수포인터의 형태 `type (*var_name)(args)`가 아니라고 판단하여 D를 찍은 것이었고, 사실 이것은 **포인터 배열**과 **배열 포인터** 사이의 관계에 대한 내용을 이해하고 있는지에 대한 문제였던 것이다. 

`int *ptr[10]` 는 연산자 우선순위 규칙에 의해 `int * (ptr[10])`으로 변환된다. 따라서 그대로 읽으면 *int * 원소 10개를 담은 ptr*이 된다.

`int (*ptr)[10]`  는 괄호의 위치가 바뀌었음에 주의하자. 

`int ptr[10]` 

`int *ptr`

```c
long arr[] = {1, 2, 3, 4, 5};
long *p = arr;
long(*ptr)[5] = &arr;
```

```
p: 6656, ptr: 6656, arr: 6656
*p: 1, *ptr: 6656, arr: 1
sizeof(p): 8, sizeof(ptr): 8, sizeof(arr): 40
sizeof(*p): 8, sizeof(*ptr): 40, sizeof(*arr): 8
```
