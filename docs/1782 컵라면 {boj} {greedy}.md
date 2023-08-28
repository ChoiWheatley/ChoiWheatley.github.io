---
aliases: 
tags: 
description:
title: 1782 컵라면 {boj} {greedy}
created: 2023-08-28T22:41:13
updated: 2023-08-28T22:43:14
---

## TL;DR

1. 데드라인으로 오름차순, 같다면 컵라면 수로 내림차순 정렬 → `p_ls`에 최대한 많은 문제를 풀 수는 있지만 많은 라면을 먹지는 못한다.
2. 컵라면 수로 minheap을 구성 → 개수가 각 과제의 deadline을 넘어가는 경우 heappop을 해서 많이 못 먹는 라면을 과감하게 포기한다.

## source code

```python
from dataclasses import dataclass
from functools import total_ordering
from heapq import heappop, heappush
from sys import stdin
from typing import Self


@dataclass
@total_ordering
class Ramyeon:
    deadline: int
    ramyeon: int

    def __lt__(self, o: Self) -> bool:
        if self.deadline == o.deadline:
            return self.ramyeon > o.ramyeon
        return self.deadline < o.deadline

    def __eq__(self, o: Self) -> bool:
        return self.deadline == o.deadline and self.ramyeon == o.ramyeon

    def __gt__(self, o: Self) -> bool:
        return o < self


if __name__ == "__main__":
    n = int(stdin.readline().strip())
    ramyeons = sorted(Ramyeon(*(int(x) for x in stdin.readline().split())) for _ in range(n))
    minheap = []
    for r in ramyeons:
        heappush(minheap, r.ramyeon)
        if len(minheap) > r.deadline:
            heappop(minheap)
    print(sum(minheap))
```
