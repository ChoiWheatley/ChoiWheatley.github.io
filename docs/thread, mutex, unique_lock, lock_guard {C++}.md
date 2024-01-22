---
aliases: 
tags: []
description: 
title: thread, mutex, unique_lock, lock_guard {C++}
created: 2024-01-21T16:23:49
updated: 2024-01-22T18:55:27
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

## mutex, lock_guard, unique_lock, scoped_lock, condition_variable

- `std::mutex`
	- `lock()`
	- `unlock()`
- `std::lock_guard<>`: 스코프 안에서만 쓸 수 있는 lock. move, copy 불가능
- `std::unique_lock<>`: move가 가능한 lock.
- `std::scoped_lock<>` (C++17~): 여러개의 mutex를 한 번에 가질 수 있어 데드락 예방에 좋음.
- `std::condition_variable` [[condition variable이란 {feat.monitor}]]에서 더 자세한 설명을 참조하세요
	- `notify_one()`
	- `notify_all()`
	- `wait(lock, pred?)` pred는 until조건임(...)
	- `wait_for(lock, duration, pred?)`
	- `wait_until(lock, time_point, pred?)`

## 22:22 공유자원에 대한 모든 접근은 lock이 선행돼야 한다.

"난 단지 읽기만 할건데요?"와 같은 안일한 생각에 mutex.lock을 안했다면 바로 Undefined Behavior이다. 유일한 예외는 constructor 또는 destructor일때만. 이 두 케이스는 오직 하나의 스레드만이 생성 또는 삭제중이어야 한다는 기본 조건이 들어있기 때문이다.

## 24:27 unlock 하기 전에 return 혹은 throw 문제

[[RAII]] 원칙을 사용하는 lock_guard를 사용하면 해당 스코프를 어떤 식으로든 벗어나게 될때 lock이 풀립니다. [[병렬성과 동시성의 차이점을 알려주세요#데드락(deadlock)에 대해 설명해주세요.|데드락에 대해 설명해주세요.]]

## 32:03 Condition Variable

[[condition variable이란 {feat.monitor}]]

```cpp
class TokenPool {
	vector<Token> tokens_;
	mutex mtx_;
	condition_variable cv_;

	/**
	 * @brief 생산자
	 */
	void returnToken(Token t) {
		unique_lock lk{mtx_};
		tokens_.push_back(t);
		lk.unlock(); // 명시적 unlock (optional)
		cv_.notify_one();
	}

	/**
	 * @brief 소비자
	 */
	Token getToken() {
		unique_lock lk{mtx_};
		cv_.wait(lk, [&](){return !tokens_.empty();}) // until not empty
		Token t = move(tokens_.back());
		tokens_.pop_back();
		return t;
	}
};
```

## 37:10 mutex + condvar는 언제 쓰나요

consumer - producer 관계에서 씁니다. consumer는 버퍼가 원소를 가지고 있을때까지 기다리고, producer는 버퍼가 full이 아닐때까지 기다립니다.

만약 오직 한 번만 맺는 관계일 경우, **promise**와 **future**를 쓰세요.

> such as Token Pool, Task Queue, Go-style channel
