---
description:
aliases: 
tags: 
created: 2023-05-25T15:10:01
updated: 2023-07-15T21:33:03
title: priority queue - python
---
- [heapq](https://docs.python.org/3/library/heapq.html#module-heapq)
- [queue.PriorityQueue](https://docs.python.org/3/library/queue.html#queue.PriorityQueue)

[[network_delay_time]] 문제를 풀다가 대가리가 아파서 조사를 진행함.

# heapq

얘는 좀 멍청하다. 무조건 min heap만 지원하고 별도의 comparator를 지원하지 않는다. 따라서 내가 특정한 키를 사용하여 비교를 수행하게 만들고 싶다면 무조건 래퍼 클래스를 하나 만들어야 한다. 게다가, 객체가 아니라 그냥 함수들의 뭉치라서, 내가 별도의 리스트를 만들어 첫번째 인자로 넘겨야 한다. 어차피 내부적으로 이진힙을 사용할거기 때문에 커다란 문제는 아니지만 그래도 좀 그렇다.

```python
from dataclasses import dataclass, field
from typing import Any

@dataclass(order=True)
class PrioritizedItem:
    priority: int
    item: Any=field(compare=False)
```

# queue.PriorityQueue

얘는.... heapq에서 객체지향적인 측면만 추가한 모습이다....... 따라서 comparator가 없고, 래퍼 클래스를 만들어야 한다는 것도 문제.
