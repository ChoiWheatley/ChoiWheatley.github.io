---
description:
aliases: 
tags: 
created: 2023-05-18T00:30:16
updated: 2023-07-15T21:30:21
title: closures, factory functions (python)
---

# Closures, Factory Functions

[What is function? | first-class-object? | closure?](https://shoark7.github.io/programming/python/closure-in-python)

파이썬에서 함수는 일급객체(First Class Object)이다. 일급객체의 필요조건 세가지는 다음과 같다.

1. 함수의 인자로 전달이 될 수 있어야 한다.
2. 함수의 반환값으로 전달이 될 수 있어야 한다.
3. 수정이 되고 할당이 될 수 있다.

```python
def delegator(fn, *args):
    # 2. 함수의 반환값으로 전달
    return fn(*args)


def add(a, b):
    return a + b


my_add = add  # 3. 수정이 되거나 할당이 될 수 있다.

# 1. 함수의 인자로 전달
delegator(my_add, 1, 2)
```

클로저는 다음 세 가지 조건을 만족하는 *함수*를 지칭한다.

1. 함수 안에 함수여야 한다. 즉, nested function
2. 이것을 감싸고 있는 바깥 스코프의 상태를 참조하여아 한다.
3. 이것을 감싸고 있는 함수는 반드시 이것을 반환하여야 한다.

❓뭔데이거

신기하다...! 클로저가 `factorial`과 `square` 별로 각자의 `cache`를 가지고 있다. 함수 호출을 하여 리턴을 받아냈음에도 `in_cache`의 상태가 초기화 되지 않았다.

웃긴건, 중간에 클로저 자체를 `del` 내장함수를 사용해 삭제해 버려도 새로운 클로저를 생성하지 못할 뿐이지, 이전에 생성했던 클로저와 내부상태는 아직도 메모리에 상주해 있다.

```python
def in_cache(func):
    cache = {}

    def wrapper(n):
        print(f"DEBUG ---> n: {n}, cache: {cache}")
        if n in cache:
            print("DEBUG ---> CACHE HIT")
            return cache[n]
        else:
            print("DEBUG ---> CACHE MISS")
            cache[n] = func(n)
            return cache[n]

    return wrapper


def factorial(n):
    ret = 1
    for i in range(1, n + 1):
        ret *= i
    return ret


def square(n):
    return n**2


# without using decorator, let's use closure then!
factorial = in_cache(factorial)

factorial(3)
factorial(5)
factorial(10)
factorial(15)
factorial(15)
factorial(15)

# Create new closure
square = in_cache(square)
square(3)
square(5)
square(10)
square(15)
square(15)
square(15)

# prev closure still remains valid!
factorial(15)
square(15)

# What if we literally **kill** `in_cache`??? Had prev closures affected???
# del(in_cache)  # commented because I'm using `in_cache` below
# factorial(15)
# square(15)

```

## **closure** magic member(?)

모든 함수(클로저 포함)는 `__closure__` 변수를 자동으로 가지고 있다. 이 친구는 클로저가 참조하고 있는 enclosing scope의 상태를 tuple 형태로 보관하고 있다. `__closure__` 원소는 `cell`로, 실제 값을 가리키고 있는 일종의 포인터 개념인 것 같다. 아직 잘 모르겠음.

- [stackoverflow의 답변](https://stackoverflow.com/a/23830790)
- [freevar?](https://en.wikipedia.org/wiki/Free_variables_and_bound_variables)
- `__code__.co_cellvars`
  - 함수의 로컬 변수들의 이름을 저장
- `__code__.co_freevars`
  - 함수의 자유 변수들의 이름을 저장
- `__closure__.cell_contents`
  - 클로저의 자유 변수들의 컨텐츠를 저장

```python
def times_mult(n):
    a = ["a"]
    b = 2

    def mult(x):
        a.append("b")
        b
        return n * x

    return mult


print(f"times_mult keep those local vars : {times_mult.__code__.co_cellvars}")

times3 = times_mult(3)
print("times3, a closure that is instantiated from `times_mult` has free vars.")
print("free vars are INDEPENDENT from `times_mult`. so this attr still alives!")
print(times3.__code__.co_freevars, "\n")

print(
    f"__closure__ attr gives tule of `cell` objects: \n{times3.__closure__}\n")
print(f"__closure__ object can be unpacked via `cell_contents`:")
print(f"cell_contents: {[e.cell_contents for e in times3.__closure__]}")
print(times3(4))
print(f"cell_contents: {[e.cell_contents for e in times3.__closure__]}")
print(times3(5))
print(f"cell_contents: {[e.cell_contents for e in times3.__closure__]}")

print("\nwe can combine free vars with name and its content!")
print(
    pretty(
        {
            v: c.cell_contents
            for v, c in zip(times3.__code__.co_freevars, times3.__closure__)
        }
    )
)

```

## Decorators are closure!

[[decorator - python]]  
그래서, 위에 작성한 `factorial` 함수에 데코레이터를 붙여 전역변수를 사용하지 않고도 자유변수만으로 메모아이제이션을 수행할 수 있게 되었다. 개쩐다!!!

```python
@in_cache
def factorial2(n):
    if n <= 1:
        return 1
    return n * factorial2(n - 1)


factorial2(3)
factorial2(3)
factorial2(4)
factorial2(10)
factorial2(9)

```

더군다나, `in_cache`는 그 자체로 구체적인 사항을 가진 클로저가 하나도 없기 때문에(오직 메모아이제이션만) 다른 알고리즘을 구현한 함수에서도 아주 간편하게 메모아이제이션을 사용하도록 그 저시기를 바꿀 수 있다.

```python
@in_cache
def sum_zero_to(n):
    if n < 1:
        return 0
    return n + sum_zero_to(n - 1)


sum_zero_to(5)
sum_zero_to(5)
sum_zero_to(10)
sum_zero_to(9)
```
