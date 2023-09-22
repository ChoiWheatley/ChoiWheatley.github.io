---
aliases: 
tags: 
description:
title: synchronization {pintos}
created: 2023-09-22T16:52:18
updated: 2023-09-22T23:02:38
---
- [[kaist pintos assignment specification {casys-kaist.github.io}]]
- [[0015 OS {ssu2021-2nd} 💻|OS]] | [[Synchronization {2021OS}]]
___

## README

> 코드 먼저 읽으세요 - 코치

코드를 먼저 읽고, 흐름을 이해하자. 동기화 기본(semaphore, lock, condvar, optimazation barriers)들에 대해서 이해가 필요하다.

## DUMPS

- preemptible kernel (pintos) & nonpreemptible kernel (traditional UNIX)의 차이점
- NMI (Non-Maskable Interrupts)란? 응급 상황에서(서버실에 불이 났을 때)만 사용되는 인터럽트가 중간에 중단되지 않는 인터럽트를 의미.
- **mutex** as MUTual EXclsion, 상호배제라는 뜻임.
	- Mutual Exclusion with Test-and-Set
- **semaphores**
	- 세마포어 연산들 (`sema_down`, `sema_try_down`, `sema_up`)은 기본적으로 원자성을 유지하기 위해서 인터럽트를 끄고 진행하는구나. thread blocking/unblocking도 끈대.
- **locks**
	- semaphore + *owner*
	- struct lock
		- holder: owner thread
		- semaphore: 이진 세마포어
- **monitors**
	- owner: acquire monitor lock and get into the monitor
		- owner can read, write protected data
	- owner: release monitor lock and get out of the monitor
	- *condition variable*: ?? waits until condition variable comes true
		- signals the condition or broadcasts the condition to wake all of them
