---
aliases: 
tags: 
description:
title: context switching 방법, 나온 이유
created: 2024-01-08T20:50:44
updated: 2024-01-08T20:51:07
---

- [[0012 Career 💼]]
- [[week07-10 {swjungle} {pintos}]]
---
여러 프로세스와 스레드들을 동시에 실행시키기 위해서 고안됐습니다. CPU burst time이 긴 스레드에 의해서 반응성이 낮아보이는 것을 막을 수 있습니다.

> 어떤 스케줄링 알고리즘이 있나요?

스레드를 중간에 실행을 중단하고 다음 스레드를 실행시키는 선점형 스케쥴링 방식, 