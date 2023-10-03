---
aliases: 
tags: 
description:
title: priority-donate-sema {swjungle}{pintos}
created: 2023-10-03T13:17:50
updated: 2023-10-03T13:28:00
---

## README

본 테스트 케이스는 waiters에 들어있던 스레드들이 어떤 리스트에 추가될 때 우선순위를 기준으로 정렬이 되면 안되는 이유에 대해서 설명합니다.

`tests/threads/priority-donate-sema.c` 테스트 파일의 의도는 H 스레드가 L이 acquire한 lock을 기다리며 기부를 하는데 L이 해당 lock을 해제하는 순간 기부가 풀려버리며 자신의 원래 우선순위로 돌아가는 데에 발생하는 엣지 케이스를 제대로 처리하는지를 검사합니다.

