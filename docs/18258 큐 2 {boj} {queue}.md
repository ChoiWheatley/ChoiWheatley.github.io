---
aliases: 
tags: 
description:
title: 18258 큐 2 {boj} {queue}
created: 2023-08-18T16:34:45
updated: 2023-08-18T16:36:52
---
<https://www.acmicpc.net/problem/18258>

## 시간초과 체크리스트

- `stdin.readline()`을 썼는가? 지금 입력이 무려 2M개가 들어온다.
- `deque`과 같은 효율적인 자료구조를 사용하였는가? 일반 `list`를 사용하면 `pop(0)` 에 O(N) 시간이 소요된다.
- 모든 명령어를 별도의 공간에 할당하지는 않았는가? 2M개의 문자열을 메모리에 다 집어넣는 것만으로도 시간제한에 걸린다.
