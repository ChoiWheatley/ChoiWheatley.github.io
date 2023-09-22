---
aliases: 
tags: 
description:
title: Synchronization {2021OS}
created: 2023-09-22T22:22:12
updated: 2023-09-22T22:27:36
---
- 두 가지 문제를 해결해야 동기화를 성공적으로 할 수 있다
	- Mutual Exclusion: 상호배제
	- Bounded Waiting: 여러 실행흐름들은 제한시간 안에 다시 실행될 수 있어야 한다.
- User Program이 Interrupt를 컨트롤 하는것이 바람직하지 않은 이유: 악의적 사용이 가능하다. 커널 코드 실행을 차단하고 영원히 자기 코드만 실행하게 만들 수도 있다.
- [?] 하드웨어적으로 동기화를 위한 ISA가 마련됐다는데? 동기화 ISA 지원은 HW lock과는 다른가?
	- Test and Set: `BTS`라고, Bit Test and Set의 약자이다. 386 시절부터 지원.
	- Swap: `BSWAP`이라고, Byte Swap의 약자이다. 486 시절부터 지원.
	- 하지만 한계가 명확. Mutual Exclusion은 되는데, Bounded Waiting은 User Program 차원에서 보장해야 함. ==> 그래서 *Semaphore*가 탄생함.

## Semaphore

두 개의 원자적 연산을 가지는 정수 변수

- Wait, Down, P 모두 Critical Section으로 들어갈 때
- Signal, Up, V 모두 Critical Section에서 빠져나올 때
