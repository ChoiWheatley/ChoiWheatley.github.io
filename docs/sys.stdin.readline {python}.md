---
aliases: 
tags: 
description:
links:
status:
title: sys.stdin.readline {python}
created: 2024-12-30T20:34:49
updated: 2024-12-30T20:36:31
---
백준에서 문제를 풀때 `input()`으로 입력값을 받아 풀때 간혹 타임아웃이 나는 경우가 있다. 이 경우 `sys.stdin.readline`을 활용해보면 해결이 될 수도 있다.

```python
from sys import stdin

input = stdin.readline # 일부로 호출하지 않고 함수 레퍼런스를 덮어씌움

input().strip() # 이유는 모르겠지만 strip을 해줘야 인풋 에러가 안나더라고.
```
