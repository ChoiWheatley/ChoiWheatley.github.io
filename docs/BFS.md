---
aliases: 
tags: 
description:
title: BFS
created: 2023-12-30T15:06:12
updated: 2023-12-30T15:11:26
---
- [[0011 Algorithms ♾️|algorithms]]

## Snippet

```python
from collections import deque

visited = [False for _ in range(n)]
dq = deque([a]) # starting point

while dq:
	cur = dq.popleft()
	visit(cur)
	for v in adj_dict[cur]:
		if not visited[v]:
			visited[v] = True
			dq.append(v)
```

## 관련 문제

- [백준-1939-중량제한](https://boj.kr/1939) 이분탐색 + BFS로, 어떤 무게 W로 물건을 배달할 수 있는지 여부를 검사한다. 이때 A -> B로 가는 경로를 찾기 위해 BFS를 사용한다.
