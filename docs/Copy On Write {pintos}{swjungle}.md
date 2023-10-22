---
aliases: 
tags: 
description:
title: Copy On Write {pintos}{swjungle}
created: 2023-10-22T05:50:32
updated: 2023-10-22T14:48:02
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

## cow-simple은 통과하는데...

hidden test case가 존재하지만 정확히 어떤 부분을 컨트롤해야 하는지 모르겠다. 지금 커널모드의 WP(Write Protection) 비트도 꺼져있어 반쪽짜리 COW임이 분명하고 권 교수님 말씀대로 구조를 뜯어고친것도 아닌데 테스트케이스가 통과하는 것이 매우 의심스럽다. 숨겨진 테스트 케이스를 알아내기 위해서 질문방에 글을 올렸으나, 조교진들도 알 수 없다는 답변에 좀 더 구체적인 힌트를 요구하기 위한 질문을 작성하려고 한다.

- cow-simple에 이은 구체적인 개선방향을 생각해봤습니다. 먼저 
