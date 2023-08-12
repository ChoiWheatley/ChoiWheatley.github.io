---
aliases: 
tags: 
description:
title: N-Queen {boj}
created: 2023-08-12T22:19:59
updated: 2023-08-12T22:33:14
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

		row_visited, diag1_visited, diag2_visited check
		
		count += solve(col + 1)

		row_visited, diag1_visited, diag2_visited uncheck

	return count
```

근데 이 방법으로는 시간초과가 발생하기 때문에 아예 다른 방식으로 접근하여야 한다.
