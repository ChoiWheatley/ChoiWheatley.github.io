---
aliases: 
tags: 
description:
title: upage vs kpage vs physical memory {pintos} {swjungle} {qna archieve}
created: 2023-10-15T19:23:41
updated: 2023-10-15T19:24:19
---
- [[week09 - Virtual Memory {pintos} {swjungle}]]
___
- 학생: 안녕하세요! 페이지 할당기 부분에서 언급되는 유저풀과 커널풀은 가상 메모리영역에서 논리적으로 구분되는 영역인지, 물리 메모리의 영역인지 질문드립니다!  
    palloc_get_page 함수를 보면 ” Obtains a single free page and returns its kernel virtual address.” 라고 되어있는 것을 보면 가상메모리의 ‘페이지’를 할당한다고 생각되어 커널풀/유저풀이 가상메모리상의 공간이라고 생각되었는데, 그렇다면 이들은 어디에 존재하고 있는건지에 대해 궁금합니다!
    
- 조교: 안녕하세요 조교 XXX입니다.  
    **user pool과 kernel pool은 물리 메모리에서 나뉘는 메모리 영역입니다.** Pintos의 경우 전체 physical memory를 50:50으로 나누어서 user pool과 kernel pool로 지정해 놓았습니다.  
    “Obtains a single free page and returns its kernel virtual address”에서 “kernel virtual address”를 설명하려면 우선 pintos에서 virtual memory를 설명해야 합니다. 그림에 보이듯이 virtual memory에서 **KERN_BASE** (PHYS_BASE라고 보이는데 이것은 구버전 pintos 기준입니다) **위의 영역은 커널만 접근할 수 있는 virtual address**이며, 이 부분의 매핑은 `(virtual address) - KERN_BASE = (physical address)`**라는 매우 단순한 1:1 매핑**이 되어 있습니다. 그러므로 palloc.c에 적혀 있는 설명에서 returns its kernel virtual address 라는 말은, physical address에서 KERN_BASE 만큼 더한 값을 리턴한다는 뜻이고, 그러므로 page in/out이 필요한 virtual page를 할당하는 것이 아니라 바로 접근이 가능한 **physical page를 할당해준다고** 생각하시면 됩니다.  
	
![[pintos-phy-mem-1.png]]