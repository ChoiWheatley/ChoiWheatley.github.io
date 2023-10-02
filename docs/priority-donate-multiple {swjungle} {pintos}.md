---
aliases: 
tags: 
description:
title: priority-donate-multiple {swjungle} {pintos}
created: 2023-09-29T22:08:20
updated: 2023-10-02T20:52:30
---
- [[week07-09 {swjungle} {pintos}]]
___

## diagram

[[priority-donate-multiple.excalidraw]]  

이번 프로젝트에서 ready list와 lock semaphore waiters list, destruction_req는 이미 `thread::elem` 멤버에 의해 자리가 보장되어있다. (왜 두 리스트에 대해서 하나의 멤버만으로 사용이 가능한지는 [다음 이슈](https://github.com/ChoiWheatley/swjungle-week07-09/issues/13)를 확인하라.) 하지만 우리가 새롭게 만든 리스트인 `thread::donate_list`는 언제나 blocked 상태인 스레드들이 들어가기 때문에 단순히 `thread::elem` 멤버를 사용했다간 연결 관계가 덮어씌워져 버린다.

![[Pasted image 20230929221915.png]]

위의 도표는 `thread::d_elem` 멤버변수를 선언하여 donate list의 원소로 사용한 모습이다. 이때 lock의 waiters 모두를 donate list에 넣지 않고 가장 큰 우선순위를 가진 스레드의 `d_elem`만 넣게 되는데, 이것이 우리 프로젝트 구현사항 중 가장 큰 특징이다. 같은 lock을 기다리고 있는 스레드들 중에서는 항상 큰 우선순위를 가진 스레드가 우선적으로 기부를 하기 때문에 굳이 다른 waiter를 기부자 목록에 올릴 필요가 없다. 덕분에 T1의 우선순위를 계산할 때 순회하는 시간이 줄어들었고, lock release할 때 같은 `wait_on_lock`을 가지로 있는 기부자 스레드만 제거하면 되어 코드의 양이 줄어든다. 이 방식으로 진행했을 때 `lock_acquire` 타이밍에 필요한 작업이 생긴다.

`lock_acquire`하려는 스레드가 lock 획득에 실패한 경우, lock holder에게 기부할지 여부를 결정해야 한다. lock holder의 우선순위보다 자신의 우선순위가 높고 이미 해당 lock의 대표 기부자보다 우선순위가 높은 경우엔 기존 기부자를 donate list로부터 삭제하고 자신을 donate list에 추가해야 한다. 다음 수도코드는 해당 작업에 대한 간략한 플로우를 설명한다.

```python
def lock_acquire(lock)
	if lock.semaphore.value == 0:
		# lock 획득에 실패
		cur_thread.wait_on_lock = lock
	
		if lock.holder.priority < cur_thread.priority:
			# do donation
			max_waiter = max(lock.semaphore.waiters)
			if list_empty(lock.semaphore.waiters):
				# 첫 빠따로 waiters 안에 들어있는 경우
				lock.holder.donate_list.append(cur_thread)
			else if max_waiter.priority < cur_thread.priority:
				# 기존 기부자를 자신으로 대치
				lock.holder.donate_list.remove(max_waiter)
				lock.holder.donate_list.insert_ordered(cur_thread)
```
