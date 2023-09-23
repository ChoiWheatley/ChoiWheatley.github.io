---
aliases: 
tags: 
description:
title: 2023-09-23 dump {swjungle}
created: 2023-09-23T18:55:55
updated: 2023-09-23T19:18:00
---

## appendix thread

- `thread_init` 새 스레드 공간 할당하는데 `thread_start` 뭐하는 놈임?
	- 부트스트랩에 의해 처음 main함수에 rip가 올라왔을때
- main에서 idle 스레드는 ready queue에 다른 스레드가 기다리고 있는지 확인하지 않고 바로 생성하는데 왜?
	- 스케줄러를 실행하기 위해서 idle 스레드를 만드는 것이다. 타이머 인터럽트 끝나고 나서 실행이 되는게 스케줄러인데, 
