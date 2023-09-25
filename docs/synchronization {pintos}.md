---
aliases: 
tags: 
description:
title: synchronization {pintos}
created: 2023-09-22T16:52:18
updated: 2023-09-25T15:45:10
---
- [[kaist pintos assignment specification {casys-kaist.github.io}]]
- [[0015 OS {ssu2021-2nd} 💻|OS]] | [[Synchronization {2021OS}]]
- `synch.c` => [Synchronization](https://casys-kaist.github.io/pintos-kaist/appendix/synchronization.html) 파트에서 다루는 다양한 개념인 semaphore, lock, condvar, optimization barriers 구현체. 
___

## README

> 코드 먼저 읽으세요 - 코치

코드를 먼저 읽고, 흐름을 이해하자. 동기화 기본(semaphore, lock, condvar, optimazation barriers)들에 대해서 이해가 필요하다.

[interrupt.c](https://github.com/ChoiWheatley/swjungle-week07-09/blob/master/threads/interrupt.c)에서 구체적인 코드를 확인하면서 읽어주세요. 이해가 되지 않는 함수에 대해서는 끝까지 파고드세요.

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
		- 모니터 안에서 cond 조건이 만족할 때 signal을 받아 탈출할 수 있도록 block한다. 즉, 일반적인 lock과는 다르게 스레드가 wait 하면 다른 스레드가 깨워주어야 한다.
	- *lock*
		- Before thread access the protected data, it first acquires the monitor lock.
		- It is then said to be "in the monitor". While in the monitor, the thread has control over all the protected data, which it may freely examine or modify. When access to the protected data is complete, it releases the monitor lock.
	- 

	```c
	/* Condition variable. */
	struct condition {
		struct list waiters;        /* List of waiting threads. */
	};
	```

- **optimizational barriers**
	- 

## semaphores

## locks

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

lock은 1로 초기화한 세마포어이다. 다만, 스레드 주인에 의해서 lock, unlock할 수 있다는 점이 단순 세마포어와 차이가 있다. 단순 세마포어는 이진일 필요가 없어 동시에 여러 스레드가 같은 데이터를 공유할 수 있다. 심지어 다른 스레드에 의해서 unlock될 수도 있다.

**`lock_acquire`**

```c
/* Acquires LOCK, sleeping until it becomes available if
   necessary.  The lock must not already be held by the current
   thread.

   This function may sleep, so it must not be called within an
   interrupt handler.  This function may be called with
   interrupts disabled, but interrupts will be turned back on if
   we need to sleep. */
void
lock_acquire (struct lock *lock) {
	ASSERT (lock != NULL);
	ASSERT (!intr_context ());
	ASSERT (!lock_held_by_current_thread (lock));

	sema_down (&lock->semaphore);
	lock->holder = thread_current ();
}
```

critical section으로 진입하기 전에 

> [!question] When to use lock or semaphore?

- 한 스레드만이 데이터를 점유해야 하는 경우, lock을 건 슬레드가 lock을 풀어야 하는 경우 lock을 쓴다.
- 위의 제약조건이 필요 없는경우, 여러 스레드들의 접근을 허용하는 경우, lock을 걸었던 스레드와 unlock하는 스레드가 굳이 일치할 필요가 없는 경우 단순 세마포어를 사용하면 된다.
