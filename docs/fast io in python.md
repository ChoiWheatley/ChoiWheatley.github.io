---
description:
aliases: 
tags: 
created: 2023-05-01T16:16:21
updated: 2023-07-15T21:33:05
title: fast io in python
---
- https://github.com/paullabkorea/programmersLv0 여기 내용을 참고했답니다.
- [python 입력](https://github.com/OrmiCodeRanger/AlgorithmCheatSheet/blob/main/sys.stdin.md)
	- 한 개의 정수 입력

		```python
		import sys
		a = int(sys.stdin.readline().rstrip())
		```

	- 정해진 개수의 정수를 한 줄에 입력받을 때

		```python
		import sys
		a,b,c = map(int, sys.stdin.readline().rstrip().split())
		```

	- 임의의 개수의 정수를 한 줄에 입력받아 리스트에 저장할 때

		```python
		import sys
		data = list(map(int, sys.stdin.readline().rstrip().split()))
		```

	- 임의의 개수의 정수를 n줄 입력받아 2차원 리스트에 저장할 때

		```python
		import sys
		n = int(sys.stdin.readline().rstrip())
		data = [list(map(int, sys.stdin.readline().rstrip().split())) for _ in range(n)]
		```

	- 문자열 n줄을 입력받아 리스트에 저장할 때

		```python
		import sys
		n = int(sys.stdin.readline())
		data = [sys.stdin.readline().strip() for _ in range(n)]
		```
