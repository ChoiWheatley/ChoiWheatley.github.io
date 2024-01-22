---
aliases: 
tags: []
description: 
title: thread, mutex, unique_lock, lock_guard {C++}
created: 2024-01-21T16:23:49
updated: 2024-01-22T17:47:28
---
- [[C++]]
- [[12. Concurrent Programming {csapp}]]
- [youtube / cppcon / back to basics : concurrency](https://youtu.be/F6Ipn7gCOsY)
---

## 목차

- C++11에 들어온 thread와 동기화 메커니즘 (mutex, condition_variable, read/write lock, once flag)
- C++20에 들어온 semaphore, latch, barrier
- 멀티 스레드에 본격적인 Promise/Future, Coroutine, ASIO, TBB
- Mutable state를 피하기 위해 나온 Blue Green Pattern
- [[병렬성과 동시성의 차이점을 알려주세요]]

## C++11 전후상황

c++11 이전까지는 플랫폼 종속적인 라이브러리 함수 (ex. pthread)를 사용했다. 하지만 C++11에 나온 표준함수를 사용하면 플랫폼 독립적으로 동시성 프로그래밍을 할 수 있게 되었다.

## 컴파일러와 atomic 타입

컴파일러는 소스코드를 최적화하여 코드를 재작성하고 위치를 옮길 수 있다. ⟹ 의도와는 다르게 배치되어 동시성 프로그래밍에서 엉뚱한 결과가 나올 수도 있음

- `atomic<>`

atomic 타입은 해당 연산에 한하여 원자적인 연산을 수행할 수 있게 해준다. 첫 번째로 컴파일러가 해당 타입에 대한 연산을 강제로 재작성하지 못하게 만들었다. 또한 메모리 배리어를 사용, 다른 스레드가 원자적인 연산을 넘어선 수행을 하지 못하게 만든다.

물리적인 데이터 경쟁으로 인한 UB는 막을 수 있지만, 그렇다고 논리적인 순서를 정의할 수는 없다.
