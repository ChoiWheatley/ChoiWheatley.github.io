---
aliases: 
tags: 
description:
title: prim algorithm과 kruskal algorithm {Minimum Spanning Tree}
created: 2023-08-20T22:13:53
updated: 2023-08-20T22:19:20
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

