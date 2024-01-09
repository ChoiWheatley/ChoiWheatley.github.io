---
aliases: 
tags: 
description:
title: lock과 semaphore와의 차이점
created: 2024-01-09T00:33:57
updated: 2024-01-09T14:42:12
---
- [[0012 Career 💼]]
- [[week07 - Threads {pintos} {swjungle}]]
---

## 대답

semaphore는 critical section에 접근하는 스레드의 순서를 보장하기 위해 만들어졌고, lock은 binary semaphore를 지칭합니다.

semaphore는 임의의 음이 아닌 정수로 초기화 가능합니다. 이 값이 0이 되기 전까지 스레드들은 세마포어의 값을 낮추면서 (Test 또는 P 라고 불림) 임계영역을 접근할 수 있습니다. 세마포어 값이 음수가 되면 그때부터 값을 낮추려는 스레드들이 waiters 리스트에 들어가 block 상태가 됩니다. 임계영역을 빠져나오는 스레드는 세마포어의 값을 올리는데 (Increment, V라고 불림) 이때 waiters에 스레드가 존재할 경우 unblock 시킨 뒤에 해당 스레드를 실행시킴으로써 로직이 흘러가게 됩니다.

## source code

[sema_down](https://github.com/ChoiWheatley/swjungle-week07-09/blob/f0ec01dc014fa1ba04b2a5011a6618cd47a83ed9/threads/synch.c#L61), [sema_up](https://github.com/ChoiWheatley/swjungle-week07-09/blob/f0ec01dc014fa1ba04b2a5011a6618cd47a83ed9/threads/synch.c#L106)
