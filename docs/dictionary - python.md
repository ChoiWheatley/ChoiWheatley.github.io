---
description:
aliases: 
tags: 
created: 2023-05-17T23:49:37
updated: 2023-07-11T15:21:09
title: dictionary - python
---

# Dictionary
- 하나만 알아두고 가자 `{}` => dictionary, `{10}` => set
- 순서가 없는 자료형, 순서를 보장하지 않는다(`ver <3.6`). 현재, dictionary는 순서를 보장한다(`ver >= 3.6`)
- 참조값은 ==가변==이다
- 다양한 자료형을 입력할 수 있다.
- 생성
	- list of tuple을 넣어도 된다. 
	```python
	another_d = dict([('one', '하나'), ('two', '둘'), ('three', '셋')])
	```
- C-family의 `switch` 처럼 구현하기
	```python
	# python ver <3.10
	def switch(day):
	    return {
	        1 : '월요일',
	        2 : '화요일',
	        3 : '수요일',
	        4 : '목요일',
	        5 : '금요일',
	        6 : '토요일',
	        7 : '일요일',
	    }[day]
	
	switch(7)
	```
- `fromkeys` 메서드는 시퀀스형 자료를 입력받아 새 `dict`를 만들어준다. 근데 `key`만 만들어주고 `value`는 `None`임. 

## Dictionary Iteration and Unpacking
- `items` 메서드를 사용하면 key-value 쌍을 모두 받아볼 수 있다. 이때, unpacking 문법을 활용하여 튜플을 받는 대신에 튜플 내용을 분해하여 데이터를 가져올 수 있는 문법이 있다.
```python
for key, value in d.items():
    print(f'key: {key}, value: {value}')
```
- unpacking을 활용한 swap
```python
a = 10
b = 15
a, b = b, a
```
