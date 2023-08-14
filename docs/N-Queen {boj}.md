---
aliases: 
tags: 
description:
title: N-Queen {boj}
created: 2023-08-12T22:19:59
updated: 2023-08-14T11:18:57
---
- [boj link](https://www.acmicpc.net/problem/9663)
- [[Doit 자료구조와 함께 배우는 알고리즘 기초 파이썬 편#8퀸 문제 ==p.204==|교재의 방법으로는 시간초과 발생함.]]

# pseudo code

```python
def solve(col):
	if col == SIDE:
		return 1
	for row in range(SIDE):
		diag1_idx = col + row
		diag2_idx = row - col + SIDE - 1
		if (row_visited[diag1_idx] or
			diag1_visited[diag1_idx] or
			diag2_visited[diag2_idx]):
			continue

		visit: row_visited, diag1_visited, diag2_visited
		
		count += solve(col + 1)

		unvisit: row_visited, diag1_visited, diag2_visited

	return count
```

근데 이 방법으로는 시간초과가 발생하기 때문에 아예 다른 방식으로 접근하여야 한다. [그런데](https://djm03178.tistory.com/m/16) pypy로 풀면 통과한다고 한다

## 신병철's idea

등록된 모든 퀸들의 `(x', y')` 과 현재 주목중인 `(y, x)`간에 `|x - x'| == |y - y'|`이라면 north east 대각선 OR south east 대각선에 의해 공격당할 위치에 놓이게 된다. 이것을 가지고 문제를 풀 수도 있다.
