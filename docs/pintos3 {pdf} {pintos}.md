---
aliases: 
tags: 
description:
title: pintos3 {pdf} {pintos}
created: 2023-10-14T04:57:15
updated: 2023-10-14T05:29:06
---
- [[Pintos_3.pdf]]
- [[week07-10 {swjungle} {pintos}]]
---

## TLDR

> 페이지 테이블이 demand paging을 지원하기 위해서 spt를 구현하라. eviction policy를 담기 위한 mechanism인 frame table을 구현하라. OS의 swap파일의 현황을 체크하기 위하여 swap table을 구현하여라. 

- Things to do
	- supplemental page table
	- physical frame management **frame table**: global data structure that keeps track of physical frames that are allocated / free
	- modifying the page fault handler for lazy loading (demand paging)
		- stack growth
		- file-mapped **file-mapped table**
	- mmap, munmap
	- swap in/out **swap table**

- beware synchronization
	- swap table은 OS 전역적으로 관리되기 때문에 동기화 메커니즘이 필수로 들어간다. file lock을 해야하나?
	- page in/out을 관리하는 구조체(frame table)은 여러 스레드가 동시에 건들일 수도 있다.

## Supplemental Page Table

두 가지 목적이 있다.

- 이미 알고있던 내용인 page-in 관련 내용.
- process exit할 때 커널이 해제시킬 페이지를 선택하는 데에도 활용됨.

## Frame Table

- [?] file system ≠ swap인건가? binary 파일은 file system으로 page-out되고 일반 파일은 swap 파일로 swap-out 되는건가?
- accessed bits & dirty bits
- eviction policy

## Swap Table

bitmap 냄새가 난다. 

> swap slot: 현재 스왑된 페이지가 해당 공간을 점유하고 있는지 여부를 저장.

스왑파일은 페이지 단위로 관리가 되겠지? 블럭 단위도 페이지 단위와 일치하는지 알아야 한다. 

## Stack Growth

계속된 함수 호출로 인해 스택이 늘어나게 되면 페이지 폴트가 발생하게 될 것이다. 이 경우 적절한 스택 접근의 여부인지 확인할 필요가 있다. 현재로서는...

- 접근 타입이 `VM_ANON`일 것
- `pg_round_up`한 결과의 페이지 주소가 spt에 등록이 되어있을 것.

## Memory Mapped Files

파일을 유저공간에 매핑하는 함수. 문제는 파일 적재가 게으르게 이루어진다는 점. 파일의 특성상 evict, process termination, 상황에서 파일에 write가 이루어져야 한다.

## Where to page out / evict to?

- `VM_ANON`: swap에 들어간다
- `VM_FILE`: 파일 시스템에 들어간다. 근데 dirty bit를 보고 write 할지 단순히 dealloc할지 결정.

> [!note] frame table에 들어가야 할 정보: seek할 위치

## suggested order

1. frame table (why???)
2. spt, page fault handler (lazily load code / data segments via page fault handler). 여기까지 하면 Project2때 했던 모든 테스트가 통과할 것임.
3. stack growth, mapped files, page reclamation
4. eviction with synchronization
