---
aliases: 
tags: 
description:
title: priority-donate-sema {swjungle}{pintos}
created: 2023-10-03T13:17:50
updated: 2023-10-03T13:52:07
---
- [[week07-09 {swjungle} {pintos}]]
___

## README

본 테스트 케이스는 ready list 또는 waiters에 들어있던 스레드들이 어떤 리스트에 추가될 때 우선순위를 기준으로 정렬이 되면 안되는 이유에 대해서 설명합니다.

`tests/threads/priority-donate-sema.c` 테스트 파일의 의도는 H 스레드가 L이 acquire한 lock을 기다리며 기부를 하는데 L이 해당 lock을 해제하는 순간 기부가 풀려버리며 자신의 원래 우선순위로 돌아가는 데에 발생하는 엣지 케이스를 제대로 처리하는지를 검사합니다.

## FLOW

1. main이 lock과 sema를 모두 init하고 그 중에서 sema의 값을 0으로 초기화 한다. (`lock.sema != sema`) L 스레드를 새로 만든다.
2. 낮은 우선순위를 가진 스레드 L이 lock을 acquire한 뒤에 sema를 down한다. 0이므로 L은 block 상태가 되고 main 스레드가 H 스레드를 만든다.
3. 높은 우선순위를 가진 스레드 H가 lock을 acquire 하지만 L이 lock holder이므로 기다려야 한다. 따라서 H는 L에게 자신의 우선순위를 기부하고 block 상태가 된다. main 스레드가 M 스레드를 만든다.
4. L보다 크고 H보다 작은 우선순위를 가진 스레드 M이 sema를 down한다. 0이므로 M은 block 상태가 되고 main 스레드로 실행권한이 넘어간다.
5. main 스레드는 sema up을 한다.

	이제 제일 먼저 뛰쳐나올 스레드는 L일까, M일까, H일까?

6. L이 높은 우선순위를 가지고 있으므로 L이 튀어나와야 한다.

**쟁점포인트**

여기에서 중요한 점은 L이 기부받은 높은 우선순위로 인해 튀어나오기 전에 `sema.waiters`를 정렬해서 넣었더라면 M 스레드가 앞에 서 있다는 것이다. 따라서 언제 어느 상황에 스레드의 우선순위가 바뀔지 모르기 때문에 사실 정렬해서 넣을 필요가 없게된다.

![[슬라이드12.png]]

![[슬라이드13.png]]
