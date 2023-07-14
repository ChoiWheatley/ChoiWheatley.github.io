---
description:
aliases: 
tags: 
created: 2023-05-17T17:39:04
updated: 2023-07-11T15:21:09
title: decorator - python
---

# Decorator

오늘 진도 엄청나게 많이 뺐네

정의:
	데코레이터는 함수의 기능을 확장하기 위해 만들어졌으며, 페이로드를 정제하는 일, 함수 호출 앞/뒤로 무언가 해야할 일이 있을 때 요긴하게 사용된다.

먼저 실무에서 데코레이터를 어떻게 쓰는지 확인해보자.
```python
@login
def 비밀게시판():
	"""
	함수를 실행하기 전에 로그인 여부를 조사한다.
	"""
	return render()

@preprocess
sum([10, '', '20', '30', None, 'hello'])
# 숫자만 정제하여 `sum`에게 넘겨주는 일을 `@preprecess`가 하게 된다.
```

데코레이터는 사실 함수인데, 함수를 인자로 받아 함수를 리턴하는 이상한 녀석이다. 이것도 전략패턴이라고 봐야할까? 얘의 `wrap_func`를 잘 보면 안에서 `func`를 호출하는데 이녀석이 바로 데코레이터를 붙인 함수이다.
```python
# Decorator 선언
def print_hello(func):
    def wrap_func():
        print("func <<print_hello>>::<<wrap_func>>")
        func()
    return wrap_func

# Decorator 호출
@print_hello
def func1():
    print('inside <<func1>>')

func1()

# Decorator 없이도 못할 건 없는데...
def func2():
    print('func <<func2>>')

print_hello(func2)()
```

데코레이터에도 인자가 들어갈 수 있다.

```python
# 데코레이터에 argument를 넣는 방법
def deco1(name):
    def deco2(func):
        def wrapper():
            print("decorator1")
            func()

        return wrapper

    return deco2


# 데코레이터를 여러 개 지정
@deco1("hojun")
def hello():
    print("hello")


hello()


# 2중 decorator
def decorator1(func):
    def wrapper():
        print(f"deco1 > wrapper > func : {id(func)}")
        func()

    print(f"deco1 > wrapper : {id(wrapper)}")
    return wrapper


def decorator2(func):
    def wrapper():
        print(f"deco2 > wrapper > func : {id(func)}")
        func()

    print(f"deco2 > wrapper : {id(wrapper)}")
    return wrapper


# 데코레이터를 여러 개 지정
@decorator1
@decorator2
def hello():
    print("hello")


hello()
```

아래는 전처리를 수행하는 데코레이터에 대한 스니펫이다.

```python
def 전처리(func):
    def wrap_func(iterable):
        return func(list(map(int, iterable)))

    return wrap_func


@전처리
def 평균(l):
    return sum(l) / len(l)


평균(["1", 2, 3, "4"])
```

# 실습 : list 전처리

```python
# 데코레이터 실습 문제
# 다음 값이 들어갔을 때, 숫자만 모두 더하는 코드를 완성하세요.
ls = ["10", True, False, "21", 0, 10, 20]


def 전처리(func):
    def wrap_func(iterable):
        iterable = map(int, filter(lambda x: type(x) is int, iterable))
        return func(iterable)

    return wrap_func


@전처리
def custom_sum(l):
    return sum(l)


print(custom_sum(ls))

assert isinstance(True, int)
assert isinstance(False, int)
```