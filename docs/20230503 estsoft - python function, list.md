---
description:
aliases: 
tags: 
created: 2023-05-03T13:04:29
updated: 2023-07-11T15:21:10
title: 20230503 estsoft - python function, list
---
- 5월 20일부터 책 집필을 할 수 있는 기회가 있을 예정. 팀이던 뭐던. 장고는 아직 진도를 안 나갔으니까 ^alg2p3
	- [[pyscript]]: 국내 책이 전무함
	- python: 라이브러리 레시피

# Function
- [콜랩 링크(이제 하나의 콜랩으로 전부 활용할거임)](https://colab.research.google.com/drive/1gxoD01mjta80MkTOlrei1BHSUI0_k9-R#scrollTo=L60Q1h-FDVHD&line=2&uniqifier=1)
- [파이썬 네이밍 규칙 - 블로그](https://dowtech.tistory.com/38)
- 

버그나 잘못된 입력에 강인한 함수를 만들어 오래오래 쓸 수 있도록 만드는 것이 함수 만들기의 관건! 수업 끝나고 돌아보니 사실 인자에 타입을 명시하기만 하면 되는 문제인 것 같기도 하고. 

타입을 명시한다고 반드시 그 타입만 들여보내는 게 아니었다. 그냥 사용자 편의 조각에 불과하고, 사실 다른 타입 변수를 넣어도 에러 없이 들어가 최후의 상황에 터진다... 💣💣💣
```python
def plus(num1, num2):
	try:
		return num1 + num2
	except:
		return float('inf')
```

## error handling with parameters
이를 막기 위해서 프로그래머는 몇가지 선택권이 주어진다(회사 컨벤션이 있다면 없을지도)
1. [`isinstance`](https://docs.python.org/3/library/2to3.html?highlight=isinstance#to3fixer-isinstance) 를 활용한 타입체크
2. [`pydantic`](https://bskyvision.com/entry/python-Pydantic-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-data-class%EB%B3%B4%EB%8B%A4-%EB%8D%94-%EB%82%98%EC%9D%80-%EB%93%AF) 모듈을 활용한 미연에 발생할 수 있는 타입에러 잡아주기 | [pydantic 공식문서](https://pydantic.dev/)
3. 적극적인 `try`, `except`구문 활용

```python
def plus_with_type(num1: int, num2: int) -> int:
    if isinstance(num1, int) and isinstance(num2, int):
        return num1 + num2
    raise TypeError('only int values are allowed')
```

위니브 대표님의 의견은 파이썬이 파이썬할 뿐이라는 것 같다. 각자 고유한 패러다임이 있는거고, 그걸 가지고 다른 언어와 비교하고 까내리는 것은 삼가자. 
> 타입힌트는 나중에 하지만 강제하진 않습니다. 저는 개인적으로 typehint도 견고한 코드를 위해 좋지만, pythonic함은 감소된다 생각해요.

## 재귀함수
아래 코드는 재귀를 활용한 (효율적인) 지수 연산이다. 
```python
def my_pow(num1, num2):
    if num2 == 0:
        return 1
    if num2 % 2 == 0:
        half = my_pow(num1, num2 // 2)
        return half * half
    else:
        half = my_pow(num1, num2 // 2)
        return half * half * num1

# test
for exp in range(40):
    print(f'2 ** {exp} = {my_pow(2, exp)}')
    assert 2 ** exp == my_pow(2, exp)

```

## Closure, 함수가 함수를 리턴한다

아래는 지수 연산을 수행하는 또 하나의 방법을 표현한다. Factory Function이라고도 불리우는 이 방식은, 휘발되지 않는 `x` 를 가지고 함수를 만든 뒤에 나중에 가서 호출하는 방식으로 이루어진다.
```python
def 제곱(x):
    def 승수(y):
        return x ** y
    return 승수


pow3 = 제곱(3)
pow3_4 = pow3(4)
```

## Local variables and Global variables

1. 전역변수 선언 시 함수 내에서 접근은 가능하지만 수정이 되지 않는다.
2. 전역변수를 함수 내에서 수정하기 위해선 `global` 을 작성하면 된다. 실무에서 거의 사용 안한다고 하넹

함수 내에서 사용되는 로컬 변수들의 변수명, 현재 값 등을 알고 싶을 때 `locals` 내장함수를 사용할 수 있다.
```python
def one():
    x = 1
    def two():
        print(x)
        print(locals())
    print(locals())
    two()
one()
```

# Lists
- 순서를 가진 데이터들의 집합 (Sequentially)
- 리스트는 값의 변경이 가능함
- 심지어 다른 자료형을 집어넣을 수도 있다.
- 심지어 리스트 안에 다른 리스트를 만들 수 있고, 그걸로 행렬을 만들 수도 있다.

컨벤션 자료형(dict, tuple, set, list)의 경우 같은 값이 있을 경우 같은 곳을 가리키게 설계되어 있다.

```python
l = [1, 999, 999, 999, 1, 1, 999, 1]
assert id(l[0]) == id(l[4]) == id(l[5])
assert id(l[1]) == id(l[2]) == id(l[3]) == id(l[6])
```

## List Operations and Methods

- 곱셈연산 `*`: 이놈은 조금 많이 이상하다. 리스트 원소의 값이 같으면 같은 메모리를 가리키도록 설계되어있기 때문에 `*` 연산을 활용한 리스트 초기화에는 각별한 주의가 필요하다. | [python list by value not by reference](https://stackoverflow.com/questions/8744113/python-list-by-value-not-by-reference) | [create list of single item repeated N times](https://stackoverflow.com/a/3459131) 
	- if you want a list, use `[e] * n`. If you want to generate the elements lazily, use `repeat`.
	- To create [unique empty sub-lists](https://stackoverflow.com/a/7924716/1048186), use for-comprehension: `[[] for i in range(0,n)]`
```python
# Those are acceptable in my mind
x = [10] * 3
print(x)
x[0] = 999
print(x)

# But things get weird
x = [[10] * 2] * 3
print(x)
# [[10, 10], [10, 10], [10, 10]]
x[0][0] = 999
print(x)
# [[999, 10], [999, 10], [999, 10]]

# What on earth? I meant the result would be [[999, 10], [10, 10], [10, 10]]
# but when you look into ids,,,
assert id(x[0][0]) == id(x[1][0]) == id(x[2][0])
```

- 덧셈연산 `+` : 얘는 `extend`와 완전히 동일하다. 즉, iterable한 녀석을 뒤에 붙여주는 것임
- `append`: 는 `extend`와는 다르게 단일 오브젝트만을 취급한다.
- `clear`:는 이름 그대로 모든 내용물을 비운다.
- `copy`: 객체의 내용을 복제한다. 명심해라, ==얕은복사==라는 사실을!! 그리고 슬라이싱 문법 중 `[:]`와 정확히 일치한다. 모든 종류의 슬라이싱도 사실 얕은복사를 수행한다. ~~[[The Book -- Rust#^vpnobi|러스트의 슬라이스타입]]~~ 과는 좀 다른 전략을 취하고 있넹... 얕은복사, 깊은복사에 관해선 다음 시간에 자세히 설명한다고 했다.
- `count`
- `index`
- `insert`
- `pop`
- `remove`
- `reverse`
- `sort`
