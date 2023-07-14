---
description:
aliases: 
tags: 
created: 2023-05-18T00:03:49
updated: 2023-07-11T15:21:09
title: args, kwargs 가변인자와 가변 키워드 인자
---

`args`, `kwargs` 얘네는 사실 그냥 이름이다. 파라미터 앞에 붙어있는 `*`가 훨씬 중요하다. => ==Packing==

`*args`는 우리가 다른 언어에서도 볼 수 있는 가변인자에 대한 저시기를 보여준다.
```python
def print_args(*args):
    print('args: ', args)
    for x in args:
        print(x)

print_args(100, True, 'leehojun')
```

`**kwargs`는 신기하게 `*`이 두 번이나 붙었네? 왜일까? => 바로 `dict`를 받기 위해서이다. default argument를 사용하여 함수를 호출하게 되었을 경우, `kwargs`는 `dict` 타입이 되어 이름과 값 쌍으로 인수가 전달이 된다. 일반 인수와 혼용하고 싶다면, 일반인수의 이름을 붙이던가, 함수선언시 일반파라미터를 가변인자 왼쪽에 넣던가 해야한다.
```python
def print_kwargs(num, **kwargs):
    print('num: ', num)
    print('kwargs: ', kwargs)
    for i in kwargs:
        print(i)

print_kwargs(100, name='최승현', age=10)
```
