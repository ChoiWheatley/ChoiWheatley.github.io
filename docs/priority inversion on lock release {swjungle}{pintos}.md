---
aliases: 
tags: 
description: 새로운 lock holder가 반드시 lock waiters의 max priority임을 보장할 수 없다.
title: priority inversion on lock release {swjungle}{pintos}
created: 2023-10-03T13:52:30
updated: 2023-10-03T14:09:26
---
- [[week07-10 {swjungle} {pintos}]]
___

## README

본 문서는 3조가 알아낸 또 하나의 엣지 케이스에 대한 내용을 다룹니다. 

[[priority-donate-sema {swjungle}{pintos}]]에서 살짝 비틀었더니 새로운 테스트 케이스가 나왔습니다. 

1. 이번에는 main 스레드가 lock을 두개(각각 A lock, B lock) 생성하고 A lock에만 acquire를 합니다. L 스레드를 실행합니다.
2. L 스레드가 B lock을 acquire하고 A lock을 acquire하다가 block 상태가 됩니다. main 스레드는 H 스레드를 실행합니다.
3. H 스레드는 B lock을 acquire하고 block 상태가 됩니다. main 스레드는 M 스레드를 실행합니다.
4. M 스레드는 L이 들고있던 B lock을 acquire하고 block 상태가 됩니다. 다시 main 스레드의 차례.
5. main 스레드는 A lock을 해제합니다. 가장 높은 우선순위를 보유하고 있는 스레드 L이(∵ 기부 받았으므로) A lock holder가 되고 L이 마저 실행이 됩니다.
6. L 스레드는 B lock을 해제합니다. B lock을 기다리고 있던 스레드 H가 B lock holder가 되고 L은 기부를 잃습니다.
7. **EDGE CASE**: L의 donate list가 비어있어 M의 기부를 받지 못합니다. 이 상황에서 L보다 크고, M보다 작은 우선순위를 가진 스레드 N이 ready list에 들어오게 된다면 priority inversion이 발생하게 됩니다.

lock release시 언제나 다음 lock holder의 priority는 waiters 중에서 최고인 스레드가 나온다는 것은 변합이 없습니다. 하지만 저 우선순위는 기부받은 우선순위일 수도 있기 때문에 original priority끼리 비교했을때의 최고 스레드를 자신의 donate list에 추가하여야 합니다.
