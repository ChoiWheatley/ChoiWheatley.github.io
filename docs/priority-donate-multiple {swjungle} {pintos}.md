---
aliases: 
tags: 
description:
title: priority-donate-multiple {swjungle} {pintos}
created: 2023-09-29T22:08:20
updated: 2023-10-02T20:15:14
---
- [[week07-09 {swjungle} {pintos}]]
___

## diagram

[[priority-donate-multiple.excalidraw]]  
![[Pasted image 20230929221915.png]]

이번 프로젝트에서 ready list와 lock semaphore waiters list는 이미 `thread::elem` 멤버에 의해 자리가 보장되어있다. (왜 두 리스트에 대해서 하나의 멤버만으로 사용이 가능한지는 [다음 이슈](https://github.com/ChoiWheatley/swjungle-week07-09/issues/13)를 확인하라.) 하지만 우리가 새롭게 만든 리스트인 `thread::donate_list`는 언제나 blocked 상태인 스레드들이 들어가기 때문에 단순히 `thread::elem` 멤버를 사용했다간 연결 관계가 덮어씌워져 버린다.

donate list 안에서 사용할 `thread::d_elem`또한 하나의 donate list의 원소로 사용되어야만 한다. 위의 도표를 예로 들면 T1의 donate list에 T2가 추가로 들어있다고 가정하자. `get_priority(T1)`의 결과값은 T9의 priority인 9로 변함이 없겠지만 
