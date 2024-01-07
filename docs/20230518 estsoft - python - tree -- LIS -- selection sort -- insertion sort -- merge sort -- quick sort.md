---
description:
aliases: 
tags: 
created: 2023-05-18T09:33:14
updated: 2023-08-13T23:01:18
title: 20230518 estsoft - python - tree -- LIS -- selection sort -- insertion sort -- merge sort -- quick sort
---

# Tree

> Tree는 방향성이 없고 사이클이 없는 그래프를 지칭한다.

- depth에 대한 개념
- bin tree
	- saturated bin tree = every single node has exactly two children
	- complete bin tree = all levels of nodes are filled except leaf nodes which are filled from the leftmost node. ==> When you implement Binary Tree with array, You'll always get complete binary tree! 😆 

## 트리 구현

![[Pasted image 20230518131309.png|400]]

제네릭 코드를 짜기 위해서 [typing.TypeVar](https://docs.python.org/3/library/typing.html#typing.TypeVar)를 찾아볼 필요가 있다. [[generic using typing.TypeVar (python)]]

# LIS (Longest Increasing Subsequence)

[[LIS 가장 긴 증가하는 부분수열]]  
<https://choiwheatley.notion.site/LIS-d61b26f5619a4ea1b0412155535ec812> 에 정리해두긴 했으나 수정할 사안이 존재하여 다시 불러왔다.  
^4ygbua

# Sort

## selection sort

간단하다. `min` 값을 가진 원소의 인덱스를 최종 리스트에 넣고 해당 원소를 방문처리한다. 남은 리스트의 원소들 중에서 다시 `min`을 찾아 최종 리스트에 하나씩 추가한다. 사실상 버블 소트만큼 오래 걸림. 얘와의 차이점이라면, 버블소트는 추가공간을 사용하지는 않는다는거.

```python
def 최솟값_인덱스_리턴_함수(리스트):
    최솟값인덱스 = 0
    for 증가값 in range(0, len(리스트)):
        if 리스트[증가값] < 리스트[최솟값인덱스]:
            최솟값인덱스 = 증가값
    return 최솟값인덱스


def 선택정렬(리스트):
    최종결과값 = []
    while 리스트:
        최솟값_인덱스_저장 = 최솟값_인덱스_리턴_함수(리스트)
        최솟값 = 리스트.pop(최솟값_인덱스_저장)
        최종결과값.append(최솟값)
    return 최종결과값


주어진리스트 = [199, 22, 33, 12, 32, 64, 72, 222, 233]

print(선택정렬(주어진리스트))
```

## insertion sort

도서관에서 책을 정렬할 때 사용하는 기법. 두서 없이 흩뿌려진 책들로부터 정렬된 결과물을 만들어 나가는데, 중요한 점은 **이분탐색**을 이용하여 빠르게 삽입할 위치를 찾아 나선다는 것이다. 파이썬에서의 이분탐색은 [[lower bound with bisect_left (python)]]을 사용하면 된다.

```python
from bisect import bisect_left


def insertion_sort(data):
    """insertion sort with binary search => O(N LOG(N))"""

    result = []

    for e in data:
        idx = bisect_left(result, e)
        if idx == len(result):
            result.append(e)
        else:
            result.insert(idx, e)

    return result


ls = [199, 22, 33, 12, 32, 64, 72, 222, 233]
assert sorted(ls) == insertion_sort(ls)
```

^fyzoll

## merge sort

[[merge sort]]

## quick sort

[[퀵정렬 {quick sort}|Quick Sort]]

괜히 겁 먹을 거 없다. 아무 원소나 하나 집어서 (처음이던, 마지막이던, 랜덤이던) 먼저 정복하고(피벗을 중심으로 정렬 상관없이 좌, 우 배치) 나중에 분할(재귀호출)하면 그것이 끝이다. 리스트 끼리 덧셈을 지원하기 때문에 `left + [pivot] + right` 하면 되어서 손도 깔끔하다.  
![[84c12ea600ff72a816c6672bcbd5e82d.png|100]]

```python
def q_sort_recur(ls):
    if len(ls) <= 1:
        return ls

    pivot = ls[-1]
    left = []
    right = []

    for elem in ls[:-1]:
        if pivot > elem:
            left.append(elem)
        else:
            right.append(elem)

    return q_sort_recur(left) + [pivot] + q_sort_recur(right)


ls = [199, 22, 33, 12, 32, 64, 72, 222, 233]
assert sorted(ls) == q_sort_recur(ls)

```
