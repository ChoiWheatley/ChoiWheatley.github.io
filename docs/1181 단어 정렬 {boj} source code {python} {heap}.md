---
aliases: 
tags: algo/heap 
description:
title: 1181 단어 정렬 {boj} source code {python} {heap}
created: 2023-08-14T23:37:48
updated: 2023-08-14T23:38:34
---

```python
"""max heap을 활용한 문제풀이
커스텀 `less` predicate은 금방 만들 수 있지만, 가장 중요한 정렬파트는 내가 직접
하나씩 구현해 보고 싶다."""

from dataclasses import dataclass
from typing import Any, List, Self, TypeVar, Type, Generic
from abc import ABCMeta, abstractmethod
from sys import stdin, stdout

standard_input = """13
but
i
wont
hesitate
no
more
no
more
it
cannot
wait
im
yours
"""


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


@dataclass
class CustomComparable(Comparable):
    key: str

    def __lt__(self, other: Self) -> bool:
        if len(self.key) == len(other.key):
            return self.key < other.key
        return len(self.key) < len(other.key)

    def __init__(self, string=None):
        self.key = "" if string is None else string


if __name__ == "__main__":
    trash = input()
    heap = Heap(CustomComparable)
    for word in {e for e in stdin}:
        heap.insert(CustomComparable(word.strip()))
    while len(heap):
        value = heap.peek()
        heap.pop()
        if value:
            stdout.write(value.key + "\n")
        else:
            break
```
