---
aliases: 
tags: 
description:
title: synchronization {pintos}
created: 2023-09-22T16:52:18
updated: 2023-09-25T14:12:35
---
- [[kaist pintos assignment specification {casys-kaist.github.io}]]
- [[0015 OS {ssu2021-2nd} 💻|OS]] | [[Synchronization {2021OS}]]
- `synch.c` => [Synchronization](https://casys-kaist.github.io/pintos-kaist/appendix/synchronization.html) 파트에서 다루는 다양한 개념인 semaphore, lock, condvar, optimization barriers 구현체. 
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

```c
/* A counting semaphore. */
struct semaphore {
	unsigned value;             /* Current value. */
	struct list waiters;        /* List of waiting threads. */
};
```

- **locks**
	- semaphore + *owner*
	- struct lock
		- holder: owner thread
		- semaphore: 이진 세마포어

```c
/* Lock. */
struct lock {
	struct thread *holder;      /* Thread holding lock (for debugging). */
	struct semaphore semaphore; /* Binary semaphore controlling access. */
};
```

- **monitors**
	- *condition variable*
		- *signals* the condition or *broadcasts* the condition to wake all of them
	- It is then said to be "in the monitor". While in the monitor, the thread has control over all the protected data, which it may freely examine or modify. When access to the protected data is complete, it releases the monitor lock.

	```c
	/* Condition variable. */
	struct condition {
		struct list waiters;        /* List of waiting threads. */
	};
	```

- **optimizational barriers**
	- 

## lock

**`lock_init`**

```c
/* Initializes LOCK.  A lock can be held by at most a single
   thread at any given time.  Our locks are not "recursive", that
   is, it is an error for the thread currently holding a lock to
   try to acquire that lock.

   A lock is a specialization of a semaphore with an initial
   value of 1.  The difference between a lock and such a
   semaphore is twofold.  First, a semaphore can have a value
   greater than 1, but a lock can only be owned by a single
   thread at a time.  Second, a semaphore does not have an owner,
   meaning that one thread can "down" the semaphore and then
   another one "up" it, but with a lock the same thread must both
   acquire and release it.  When these restrictions prove
   onerous, it's a good sign that a semaphore should be used,
   instead of a lock. */
void
lock_init (struct lock *lock) {
	ASSERT (lock != NULL);

	lock->holder = NULL;
	sema_init (&lock->semaphore, 1);
}
```

주석에서 중요한 내용이 들어있다. semaphore는 이진 세마포어로 쓴다면 lock과 동일하다. 다만 lock과 semaphore와의 차이점은 세마포어 자체는 그것을 관리하는 주인이 없다는 것이다. 세마포어는 단순히 
