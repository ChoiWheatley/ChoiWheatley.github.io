---
description:
title: Segment Tree
created: 2023-02-14T15:18:23
categories: 
 - 
aliases: 
 - segment tree
 - 세그먼트 트리
tags: [" algo/segment_tree ", algo/segment_tree]
date created: Tuesday, February 14th 2023, 3:18:23 pm
date modified: Monday, February 27th 2023, 6:20:45 pm
updated: 2023-07-11T15:21:07
---
parent link: [[0011 Algorithms ♾️]]

---
# ChatGPT의 설명

세그먼트 트리(Segment Tree)는 구간 쿼리(segment query)를 빠르게 수행하기 위한 자료구조입니다. 예를 들어, 주어진 배열에서 특정 구간의 합, 최소값, 최대값 등을 구하는 작업을 빠르게 처리할 수 있습니다.

수도코드로는 다음과 같이 구현할 수 있습니다:
```python
# arr: 원본 배열, tree: 세그먼트 트리, node: 현재 노드, start/end: 현재 노드가 표현하는 구간
# init, update, query는 각각 초기화, 업데이트, 쿼리 연산을 수행하는 함수입니다.

# 세그먼트 트리 초기화
def init(arr, tree, node, start, end):
    if start == end:
        tree[node] = arr[start]
    else:
        mid = (start + end) // 2
        init(arr, tree, node*2, start, mid)
        init(arr, tree, node*2+1, mid+1, end)
        tree[node] = tree[node*2] + tree[node*2+1]

# 세그먼트 트리 업데이트
def update(tree, node, start, end, index, diff):
    if index < start or end < index:
        return
    tree[node] += diff
    if start != end:
        mid = (start + end) // 2
        update(tree, node*2, start, mid, index, diff)
        update(tree, node*2+1, mid+1, end, index, diff)

# 세그먼트 트리 쿼리
def query(tree, node, start, end, left, right):
    if left > end or right < start:
        return 0
    if left <= start and end <= right:
        return tree[node]
    mid = (start + end) // 2
    return query(tree, node*2, start, mid, left, right) + query(tree, node*2+1, mid+1, end, left, right)

```

# 연습문제

- [[Segment Tree 연습문제 1]]
- [[Segment Tree 연습문제 2]]


