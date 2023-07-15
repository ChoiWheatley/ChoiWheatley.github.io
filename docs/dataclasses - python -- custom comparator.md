---
description:
aliases: 
tags: 
created: 2023-05-26T09:38:36
updated: 2023-07-15T21:30:20
title: dataclasses - python -- custom comparator
---
- https://docs.python.org/3/library/dataclasses.html

python에는 구조체가 없다. 아쉬운 사실이고, 구조체처럼 쓰기 위해선 `__init__` 메서드 안에 필드를 집어넣고 입맛에 따라 `__repr__`(format to string), `__lt__`, `__le__`, `__gt__`, `__ge__`(compare), `__hash__`(hash), 등등을 **직접** 구현해 주어야 한다는 귀찮음이 존재한다. 이것을 깔끔하게 추상화시켜주는 모듈이 바로 `dataclasses`이다. 얘는 너무나도 굉장하고 개쩔어서 마치 rust의 derive 문법처럼 내가 원하는 속성을 지정하기만 하면 알아서 위의 magic method들을 생성해준다!

아래는 비교연산을 지원하는 커스텀 클래스이다.

```python
@dataclass(order=True)
class Ord(Generic[T]):
    """Custom comparable class"""

    key: T = field(compare=True)
    value: Any = field(compare=False)
```
