---
description:
aliases: 
tags: 
created: 2023-05-01T13:06:30
updated: 2023-07-15T21:33:02
title: 함수 - python
---
- 함수
	- 파라미터와 아규먼트의 차이: 파라미터는 함수 **선언**시에 `(`과 `)` 사이에 들어가 있는 예약된 변수를 의미하고, 아규먼트는 함수 **호출**시에 `(`과 `)` 사이에 들어가는 실제 변수를 의미한다.
	- 리턴이 없는 함수는 자동으로 `None`을 리턴한다.
- 전역변수 `global`
	- 함수 스코프 안에 변수 선언 시 왼쪽에 `global`을 써 넣으면 해당 저시기는 파일 전역적으로 선언한 같은 이름의 변수와 연동???이 된다.
	- [?] 연동(이른바 포인터)이냐 아니면 진짜 실체냐?
- 함수 응용
	- nested function을 만들 수 있다. not surprised
- 연산자 우선순위

# Function

신기하게도, 타입을 명시하지 않아도 아무거나 들어간다...! 이를 사전에 막기 위해선 [`isinstance`](https://docs.python.org/3/library/2to3.html?highlight=isinstance#to3fixer-isinstance)라던가 [`pydantic`](https://bskyvision.com/entry/python-Pydantic-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-data-class%EB%B3%B4%EB%8B%A4-%EB%8D%94-%EB%82%98%EC%9D%80-%EB%93%AF) 이라는 모듈을 사용하는 방법을 고려할 수 있다. [pydantic 공식](https://pydantic.dev/)

## What is Closure?

아래는 제곱을 수행하는 또하나의 방법을 표현한다. 이것은 팩토리 함수라고도 부른다. 원래는 휘발되었어야 하는 `x`를 가진 채 `x ** y`를 수행하는 함수를 리턴할 수 있다.

```python
def 제곱(x):
    def 승수(y):
        return x**y

    return 승수


pow3 = 제곱(3)
pow3_4 = pow3(4)
```

[[closures, factory functions (python)]]  
[[generator function - python|generator]]도 클로저이다.
