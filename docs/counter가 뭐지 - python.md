---
description:
aliases: 
tags: 
created: 2023-05-17T14:19:36
updated: 2023-07-11T15:21:09
title: counter가 뭐지 - python
---
```python
from collections import Counter

def solution(array, n):
    value = Counter(array).get(n)
    return 0 if value == None else value
```