---
aliases: 
tags: 
description:
title: synchronization {pintos}
created: 2023-09-22T16:52:18
updated: 2023-09-22T22:06:50
---
- [[kaist pintos assignment specification {casys-kaist.github.io}]]
___

## README

> 코드 먼저 읽으세요 - 코치

코드를 먼저 읽고, 흐름을 이해하자. 동기화 기본(semaphore, lock, condvar, optimazation barriers)들에 대해서 이해가 필요하다.

## DUMPS

- preemptible kernel (pintos) & nonpreemptible kernel (traditional UNIX)의 차이점
- NMI (Non-Maskable Interrupts)란? 응급 상황에서(서버실에 불이 났을 때)만 사용되는 인터럽트가 중간에 중단되지 않는 인터럽트를 의미.
- **semaphores**
	- 
