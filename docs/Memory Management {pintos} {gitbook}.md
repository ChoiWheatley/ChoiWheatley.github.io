---
aliases: 
tags: 
description:
title: Memory Management {pintos} {gitbook}
created: 2023-10-15T19:51:57
updated: 2023-10-15T20:05:14
---
- [[week09 - Virtual Memory {pintos} {swjungle}]]
___
- [[supplemental page table 만들기 {pintos} {swjungle}]]
- `struct page_operations` 안에 있는 세 함수 포인터는 일종의 인터페이스로 활용된다. `VM_ANON`, `VM_UNINIT`, `VM_FILE` 종류에 따라 다른 구현체를 사용하도록 만든 케이스.
- [x] 새로운 프로세스가 시작될 때 `__do_fork`에서 호출하는 `supplemental_page_table_init`을 구현하세요.
- frame management
	- struct frame 안에 kva(kernel virtual addr)과 page(page 구조체 포인터)를 가지고 있다.
	- `palloc_get_page`에서 호출한다. 새로운 프레임을 가져오기 위해서 `frame` 정보를 활용한다. by `vm_get_frame(void) -> frame *` | swap out 구현은 뒤의 일이므로 해당 케이스에 대해서는 `PANIC("todo")` 이렇게 작성하래.
	- `vm_do_claim_page(page) -> bool` : 페이지와 프레임을 할당 (매핑?)해주는 함수. MMU가 보는 페이지 테이블의 정보를 수정해야 한다.
	- `vm_claim_page(va) -> bool`: va에 페이지를 할당하고 프레임을 할당해준다.
	- 페이지와 프레임을 분리한 덕분에 뒤에 나올 lazy loading이 가능해졌다.

> `anon_page` will contain all the necessary information we need to keep for an anonymous page

현재 `anon_page` 타입은 아무것도 없음. 여기에 anonymous page를 위한 속성을 추가해야 할 터. swap in/out 정보 말고 뭐가 더 필요한지 아직 잘 모르겠음.

![[demand-paging.jpg]]
