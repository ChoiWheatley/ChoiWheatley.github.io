---
aliases: 
tags: 
description:
title: 11279 최대 힙 {boj} {heap} {python class Heap}
created: 2023-08-19T13:11:03
updated: 2023-08-19T13:13:45
---
<https://www.acmicpc.net/problem/11279>

[[Heap]]

## python sourcecode

파이썬으로 직접 힙 클래스와 비교자를 구현해서 문제를 풀었다. 사실 이 코드는 [[1181 단어 정렬]] 에서 가져온 거임

```python
from abc import ABCMeta, abstractmethod
import sys
from typing import Any, Generic, List, Type, TypeVar


class Comparable(metaclass=ABCMeta):
    @abstractmethod
    def __lt__(self, other: Any) -> bool:
        # 제네릭 T 타입은 반드시 구현하여야 하는 메서드
        ...

    @abstractmethod
    def __init__(self):
        ...


T = TypeVar("T", bound=Comparable)


class Heap(Generic[T]):
    """완전이진트리로 구현된 힙. 인덱스는 보기 편하게 1부터 시작합니다. `__lt__`를 기준으로 minheap을 구축합니다."""

    data: List[T]

    @classmethod
    def parent(cls, i):
        return i >> 1

    @classmethod
    def left(cls, i):
        return i << 1

    @classmethod
    def right(cls, i):
        return (i << 1) + 1

    def __init__(self, t: Type[T]) -> None:
        self.data = [t()]  # 0번 인덱스는 사용안함

    def insert(self, item: T) -> None:
        data = self.data
        parent = Heap.parent

        data.append(item)

        # bubble up
        idx = len(data) - 1
        while parent(idx) > 0 and data[idx] < data[parent(idx)]:
            data[parent(idx)], data[idx] = data[idx], data[parent(idx)]
            idx = parent(idx)

    def pop(self) -> None:
        """루트 원소를 삭제하고 힙으로 다시 만든다"""
        if len(self) == 1:
            self.data.pop()
            return None
        if len(self) < 1:
            return None
        data = self.data
        left = self.left
        right = self.right

        # get last element root and bubble down
        data[1] = data.pop()

        idx = 1
        while left(idx) <= len(self):
            next_idx = 0
            if left(idx) == len(self):
                # right에 데이터가 없고 left가 마지막인 경우
                next_idx = left(idx)
            else:
                # 아래에 얼마나 더 내려가야 하는지 모르는 상황 → minheap 기준으로 작은 놈을 올려보내는 것이 상식
                if data[left(idx)] < data[right(idx)]:
                    next_idx = left(idx)
                else:
                    next_idx = right(idx)
            if data[next_idx] < data[idx]:
                # DO bubble down
                data[idx], data[next_idx] = data[next_idx], data[idx]
            else:
                # end bubble down
                break
            idx = next_idx

    def __len__(self) -> int:
        return len(self.data) - 1

    def peek(self) -> T | None:
        if len(self) == 0:
            return None
        return self.data[1]
```
