---
description:
aliases: 
tags: 
created: 2023-05-01T13:06:02
updated: 2023-07-11T15:20:17
title: 반복 - python
---
- `for`
- `while`
- `break`
- `continue`
- `pass`?
	- 단순히 실행할 코드가 없을때 적는 저시기임. 함수, 반복문에서 사용가능.
- `else`? 반복문에 `else` 키워드?
	- `for` 문 끝에 `else`를 붙이면 루프가 정상 종료 되거나 처음부터 자료형이 비어있을 때 실행한다. `break`를 만나면 `else`를 실행하지 않고 빠져나간다.
	- `while`문에도 사용가능하고 심지어 `try-except-else`로 사용할 수도 있다.
- list comprehension
	- `[i for i in range(1, 10)]`와 같은 문법을 지능형 리스트라고 말한다. map -> collect를 한 번에 해결!
- `enumerate`
	- 러스트의 [[enumerate and zip]] 의 enumerate와 같다. 인덱스 순서를 매겨주는 편리한 녀석이다.
- nested loops
- tuple unpacking
```python
for key, value in some_dict.items():
	print(f'{key} : {value}') 
```
- `in` 문법을 활용하여 순회시, 시퀀스형 자료형은 메서드로 `__iter__` 와 `__next__`를 가지고 있어야 한다. => 커스텀 시퀀스 자료형 만들기도 개꿀
- [[for-loop in python]]
- 