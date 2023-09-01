---
aliases: 
tags: 
description:
title: pointer array and array pointer {Pointer Artithmetic}
created: 2023-09-01T11:30:13
updated: 2023-09-01T22:21:54
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

`int *ptr[10]` 는 연산자 우선순위 규칙에 의해 `int * (ptr[10])`으로 변환된다. 따라서 그대로 읽으면 *int * 원소 10개를 담은 ptr*이 된다. 타입은 `int *[10]`이다. stride는 10이다.

`int (*ptr)[10]` 는 *int 원소 10개를 담은 배열을 가리키는 포인터*이다. 타입은 `int (*)[10]`이다. stride는 10이다.

`int ptr[10]` 는 *int 원소 10개를 담은 배열*이다. 타입은 `int [10]`이다. stride는 10이다. 중요한 점은 `ptr == &ptr`이라는 점이다. 자세한 정보는 [[#array pointer]]를 참고하라.

`int *ptr` 는 *int 원소를 가리키는 포인터*이다. 타입은 `int *`이다. stride는 1이다.

## array pointer and array

- [gfg](https://www.geeksforgeeks.org/pointer-array-array-pointer/)

적절한 예시들이 있기에 가져왔다. 배열을 가리키는 포인터 `ptr`는 `arr`의 주소를 값으로 가지고 있다. `*ptr`은 `arr`의 첫번째 원소의 주소를 가리키게 될텐데, 사실 두 값은 같다! 나는 처음엔 별도의 `arr`를 위한 공간과 배열 `{1, 2, 3, 4, 5}`를 위한 공간이 마련된 다음 `arr`이 배열의 첫번째 원소의 주소를 값으로 가지고 있는 것으로 생각했으나 나의 생각이 틀렸다.

다만, `arr`과 `&arr`이 완전히 같은 것은 아니다. 아래의 예시를 통해 보게되면, `sizeof(arr) = 20`인데 반해 `sizeof(&arr) = 8`이라는 점이다. 두 차이점을 궁금해 한 어떤 사람이 질문을 올렸고 <https://stackoverflow.com/a/2528328/21369350> 에서 확인해 보기 바란다.

한 마디로 정리하자면, `arr`의 stride는 1이고, `&arr`의 stride는 N이다.

- `arr`
	- `sizeof(arr) = N`
	- stride of `arr` is 1
- `&arr`
	- `sizeof(&arr)` is size of type of element
	- stride of `&arr` is N

```c
long arr[] = {1, 2, 3, 4, 5};
long *p = arr;
long(*ptr)[5] = &arr;
```

```
p: 140731009609168
arrptr: 140731009609168 # arrptr는 &arr이다
ptrarr: 140731009609200 # ptrarr는 별개의 배열이다.
arr: 140731009609168 # stride 5짜리 배열의 주소
&arr: 140731009609168 # stride 1짜리 

*p: 1
*arrptr: 140731009609168 # *arrptr는 arr이다
*ptrarr: 140731009609168 # *ptrarr는 arr이다
*arr: 1

sizeof(p): 8 # pointer 8byte
sizeof(arrptr): 8 # pointer 8byte
sizeof(ptrarr): 40 # pointer 8byte * 5
sizeof(arr): 20 # int 4byte * 5
sizeof(&arr): 8 # pointer 8byte

sizeof(*p): 4
sizeof(*arrptr): 20
sizeof(*ptrarr): 8
sizeof(*arr): 4

&arr[5]: 140731009609188
&arr + 1: 140731009609188
arrptr + 1: 140731009609188
ptrarr + 1: 140731009609208
```
