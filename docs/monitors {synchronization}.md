---
aliases: 
tags:
  - scrap
description: 
title: monitors {synchronization}
created: 2023-09-25T19:55:45
updated: 2023-09-25T20:01:37
---
- <https://en.wikipedia.org/wiki/Monitor_(synchronization)>  
- [[synchronization {pintos} {semaphore} {lock} {monitor}]]에서 조사하다가 발견한 문서
___
모니터는 다양한 조건을 감지하는 스레드를 위해서 사용할 수 있다. condition variable과 lock을 함께 쓴다. 모니터에 접근하기 위해서 lock이 사용되고, 한 번에 하나의 스레드만 모니터에 들어갈 수 있다. 모니터 안에서 데이터베이스 트랜잭션 커밋이라던가 유저 입력, 타이머 같은 어떤 조건이 만족될 때까지 condition variable이 제공하는 waiters 컨테이너에 들어가 대기상태가 된다.

![[Pasted image 20230925194140.png]]
