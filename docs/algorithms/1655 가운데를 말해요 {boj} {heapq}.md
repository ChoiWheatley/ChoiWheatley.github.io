---
aliases: 
tags: 
description:
title: 1655 가운데를 말해요 {boj} {heapq}
created: 2023-08-19T17:30:38
updated: 2023-08-19T17:36:48
---

## TL;DR

[[Heap|커스텀 힙]]을 사용하여 풀게되면 시간초과가 발생하는 것을 보고 비효율적인 구석이 있다는 사실을 뒤늦게 깨달았다... heapq 모듈 사용하니 바로 통과했다.

## 플로우

[[No. 25 중간값 구하기]] 문제의 경우 처음 하나의 값이 주어지고 그 뒤부터는 연속적으로 두개씩 값이 들어오기 때문에 홀수, 짝수 구분을 할 필요가 없었다. 하지만 이번 문제는 한 번에 하나의 값만 주어지기 때문에 짝수인 경우엔 중앙값이 두개가 된다. 이 문제에서는 짝수인 경우 작은 중앙값을 리턴하라고 했다.

일단 두 힙을 리본모양처럼 붙여 그 사이에 mid 값을 집어넣는 것은 동일하다. 그리고 다음 두 조건을 만족시키는 것 또한 동일하다.

1. leftheap과 rightheap의 사이즈를 항상 동일하게 만들어라
2. leftheap.top < mid < rightheap.top

## source code {python}

나는 배열을 사용하여 100,000개의 입력과 출력을 저장하기 싫어하는 사람이므로 [[generator function - python]]을 사용하여 문제를 해결했다.

```python
"""
가운데를 말해요
이거 swea에서 중간값 찾기와 완전 똑같은 문제임
https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV-fO0s6ARoDFAXT
두 힙을 붙여넣고 새로운 값이 추가될 때마다 중간값을 바꾸면 되는 문제. 이때 두가지 조건을 잘 지켜야 하는데,

1. maxheap.size == minheap.size
2. maxheap.peek() < mid < minheap.peek()
"""
from typing import Any, Generator, Iterable, List
from sys import stdin, stdout
from heapq import heappush, heappop


class MinHeap:
    def __init__(self, n=0):
        super().__init__()
        self.value = n

    def __lt__(self, other: Any) -> bool:
        return self.value < other.value


class MaxHeap:
    def __init__(self, n=0):
        super().__init__()
        self.value = n

    def __lt__(self, other: Any) -> bool:
        return self.value > other.value


def solve(seq: Iterable[int]) -> Generator[int, Any, Any]:
    it = iter(seq)
    leftheap: List[MaxHeap] = []
    rightheap: List[MinHeap] = []
    mid = it.__next__()
    yield mid

    for count, new in enumerate(it, start=2):
        if count % 2 == 0:
            # mid 변수에 new를 할당하지 않는다.
            # 두 중앙값은 두 힙의 루트에 존재
            if mid < new:
                heappush(rightheap, MinHeap(new))
                heappush(leftheap, MaxHeap(mid))
            else:
                heappush(leftheap, MaxHeap(new))
                heappush(rightheap, MinHeap(mid))
            mid = leftheap[0].value

        else:
            # mid 변수에 new를 할당한다. 다만, new의 위치에 따라서
            # 다양한 케이스가 존재
            if new < leftheap[0].value:
                # new go left, root of leftheap become mid
                mid = leftheap[0].value
                heappop(leftheap)
                heappush(leftheap, MaxHeap(new))
            elif rightheap[0].value < new:
                # new go right, root of rightheap become mid
                mid = rightheap[0].value
                heappop(rightheap)
                heappush(rightheap, MinHeap(new))
            else:
                mid = new

        yield mid


if __name__ == "__main__":
    n = int(stdin.readline().strip())
    gen = (int(stdin.readline().strip()) for _ in range(n))

    for i in solve(gen):
        stdout.write(str(i) + "\n")
```
