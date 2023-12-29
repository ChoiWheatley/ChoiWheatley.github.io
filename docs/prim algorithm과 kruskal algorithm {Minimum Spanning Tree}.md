---
aliases: 
tags: 
description:
title: prim algorithm과 kruskal algorithm {Minimum Spanning Tree}
created: 2023-08-20T22:13:53
updated: 2023-08-20T22:29:57
---
- [[graph 기초#최소신장트리 (minimum spanning tree)]]

최소신장트리를 만들기 위해서 먼저 프림의 알고리즘부터 이야기해보자.

## prim algorithm (pseudocode)

```python
def prim(G: n개의 정점이 있고 가중치가 있는 비방향 그래프):
	T = G에서 가장 작은 가중치를 갖는 간선 (u, v)
	for _ in range(n-2):
		e = T에 추가되었을 때 순환이 발생하지 않고 T의 정점들과 연결된 간선들 중 가장 저렴한 가중치를 갖는 간선
		T += e
	return T
```

![[prim algorithm excercise.jpg]]

## kruskal algorithm (pseudocode)

```python
def kruskal(G: 가중치를 갖고 n개의 정점을 갖는 비방향 그래프):
	T = {} # empty graph
	for _ in range(n-1):
		e = 추가되었을 때 T에 순환을 일으키지 않는 최소가중치 간선
		T += e
	return T
```


## 관련 문제

- [[1197 최소 스패닝 트리 {boj}]]