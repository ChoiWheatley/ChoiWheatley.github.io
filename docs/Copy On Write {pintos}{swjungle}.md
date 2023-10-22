---
aliases: 
tags: 
description:
title: Copy On Write {pintos}{swjungle}
created: 2023-10-22T05:50:32
updated: 2023-10-22T10:55:53
---
- [[week09 - Virtual Memory {pintos} {swjungle}]]
- [[11. memory mapping and Copy-on-write (COW) {SP}]]
___

## wiki 정리

<https://en.wikipedia.org/wiki/Copy-on-write>

PTE ⟶ mark as read-only, keep count of the **number** of the refs

- only one ref ⟶ skip new allocation
- greater than 1 ⟶ allocate new page, decrementing ref count from original page

[[Drawing 2023-10-22 06.00.07.excalidraw]]

![[Pasted image 20231022105535.png]]
