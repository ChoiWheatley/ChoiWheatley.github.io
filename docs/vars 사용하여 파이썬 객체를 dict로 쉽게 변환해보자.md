---
aliases: 
tags: 
description:
title: vars 사용하여 파이썬 객체를 dict로 쉽게 변환해보자
created: 2024-04-25T14:34:54
updated: 2024-04-25T14:37:45
---
- [stackoverflow / python dictionary from an object's fields](https://stackoverflow.com/questions/61517/python-dictionary-from-an-objects-fields)

```
>>> class A:
...     def __init__(self):
...             self.b = 1
...             self.c = 2
...     def do_nothing(self):
...             pass
...
>>> a = A()
>>> a.__dict__
{'b': 1, 'c': 2}
>>> vars(a)
{'b': 1, 'c': 2}
```
