---
aliases: 
tags: 
description:
title: dataclass ValueError mutable default class list is not allowed, use default_factory {python}
created: 2023-08-20T14:27:22
updated: 2023-08-20T14:28:11
---
원인 코드

```python
@dataclass
class Node:
    idx: int
    children: List[Self] = []
    parent: Self | None = None
```

에러메시지

```
ValueError: mutable default <class 'list'> for field children is not allowed: use default_factory
```
