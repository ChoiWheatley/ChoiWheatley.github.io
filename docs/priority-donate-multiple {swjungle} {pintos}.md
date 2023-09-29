---
aliases: 
tags: 
description:
title: priority-donate-multiple {swjungle} {pintos}
created: 2023-09-29T22:08:20
updated: 2023-09-29T22:32:35
---
- [[week07-09 {swjungle} {pintos}]]
___

## diagram

[[priority-donate-multiple.excalidraw]]  
![[Pasted image 20230929221915.png]]

T1이 동시에 여러 lock을 acquire 하는 경우는 발생할 수 있다. 이때 lock을 기다리는 스레드들 중 가장 높은 priority를 기부받아 실행될 것임은 자명하다. 그렇다면 단순히 T1이 걸어잠근 모든 lock을 기다리는 스레드들을 모두 donate_list에 넣으면 좋을까? 이러면 공간의 낭비가 발생하기도 하거니와, [[list {C}#Generic Linked List]] 에서 연결리스트의 노드가 스스로 prev, next를 저장하는 특성상 한 번에 하나의 리스트에만 들어갈 수 있다는 어처구니 없는 제약조건이 생겨버린다. 따라서 어떤 스레드가 리스트 A, 리스트 B 동시에 들어가 있으려면 적어도 두 개의 `list_elem` 타입 멤버가 필요하다. 

이번 프로젝트에서 ready list와 lock semaphore waiters list는 이미 `thread::elem` 멤버에 의해 자리가 보장되어있다. (왜 두 리스트에 대해서 하나의 멤버만으로 사용이 가능한지는 [다음 이슈](https://github.com/ChoiWheatley/swjungle-week07-09/issues/13)를 확인하라.) 하지만 우리가 새롭게 만든 리스트인 `thread::donate_list`는 언제나 blocked 상태인 스레드들이 들어가기 때문에 단순히 `thread::elem` 멤버를 사용했다간 연결 관계가 덮어씌워져 버린다.

donate list 안에서 사용할 `thread::d_elem`또한 하나의 donate list의 원소로 사용되어야만 한다. 위의 도표를 예로 들면 T1의 donate list에 T2가 추가로 들어있다고 가정하자. `get_priority(T1)`의 결과값은 T9의 priority인 9로 변함이 없겠지만 