---
aliases: 
tags: 
description:
title: Custom Comparable type {python} using `__lt__`
created: 2023-08-14T20:32:45
updated: 2023-08-14T20:34:44
---
- [sof](https://stackoverflow.com/questions/37669222/how-can-i-hint-that-a-type-is-comparable-with-typing)
- [[dataclasses - python -- custom comparator]]도 참고해 보세요

```python
from abc import ABCMeta, abstractmethod
from typing import Any, TypeVar

class Comparable(metaclass=ABCMeta):
    @abstractmethod
    def __lt__(self, other: Any) -> bool: ...

CT = TypeVar('CT', bound=Comparable)
```
