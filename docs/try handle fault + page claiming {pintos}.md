---
aliases: 
tags: 
description:
title: try handle fault + page claiming {pintos}
created: 2023-10-13T16:59:50
updated: 2023-10-13T17:43:10
---
- [[week07-10 {swjungle} {pintos}]]
- <https://casys-kaist.github.io/pintos-kaist/project3/anon.html>
- <https://casys-kaist.github.io/pintos-kaist/project3/vm_management.html>
___

## 역할

demand paging으로 인해 page fault가 발생하더라도 바로 exit을 하면 안된다. 따라서 spt를 검사하고, stack growth인지 판단하고, malicious 접근 여부인지 파악하는 루틴이 필요하다.

## claim

가상주소와 물리주소 매핑을 수행. page fault가 발생한 va를 가지고 실제 프레임을 들고있는 struct page를 page table에 연결한다.

- [?] do_claim은 알겠는데 claim 왜씀? 

`do_claim` 마지막에는 다형적인 `swap_in`을 호출하여 spt set page를 수행한다.
