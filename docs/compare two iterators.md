---
description:
aliases: 
tags: 
created: 2023-04-10T22:41:58
updated: 2023-07-11T15:21:09
title: compare two iterators
---
- https://rust-unofficial.github.io/too-many-lists/sixth-random-bits.html
- 이 페이지 하나에서 도대체 몇 가지 꿀팁을 얻어가는거야? 혜자다
- `PartialEq`을 구현하기 위한 코드에서 두 이터레이터를 비교하는 방법에 대한 코드가 나온다. 
- 나는 처음에 `zip`으로 만들어서 모든 원소들을 튜플 이터레이터로 묶은 뒤에 비교하는 과정을 수행했는데... 다음 코드를 보자.
```rust
self.iter().eq(other)
```
- 푸하하 `PartialOrd`도 똑같다. 
```rust
self.iter().partial_cmp(other)
```
- `Ord`가 빠지면 섭섭하지
```rust
self.iter().cmp(other)
```
