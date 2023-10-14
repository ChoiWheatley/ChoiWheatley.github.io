---
aliases: 
tags: 
description:
title: week09 - Virtual Memory {pintos} {swjungle}
created: 2023-10-15T04:47:57
updated: 2023-10-15T04:48726:59
---

### INDEX

- [[각종 QNA 정리 {swjungle}{pintos}{project3}]]
- [[pintos3 {pdf} {pintos}]] ⟶ Top-Down Approach
- pintos-kaist gitbook
	- [Introduction](https://casys-kaist.github.io/pintos-kaist/project3/introduction.html)
	- [FAQ](https://casys-kaist.github.io/pintos-kaist/project3/FAQ.html)
	- [한글번역 {Notion}](https://www.notion.so/PROJECT-3-VIRTUAL-MEMORY-d16fc8d04f4d4829b7e25691a235901c)
- CSAPP: [[9. Virtual Memory]]
- Yujip Won slides
	- [[2022_Pintos_Part3_VirtualMemory_01_DemandPaging 1.pdf]]
	- [[2022_Pintos_Part3_VirtualMemory_02_Swapping_etc.pdf]]

### READ

- 병철 추천 - [https://blog.xenoscr.net/2021/09/06/Exploring-Virtual-Memory-and-Page-Structures.html](https://blog.xenoscr.net/2021/09/06/Exploring-Virtual-Memory-and-Page-Structures.html)
- x86-64 isa - 한 번 훑어봄. 가상메모리와 pml4 포스팅도 확인바람. [https://it-eldorado.tistory.com/35](https://it-eldorado.tistory.com/35)  
- [eldorado.tistory.com - 가상 메모리](https://it-eldorado.tistory.com/52)
- 지난기수 질문답변 - load segment 위주로 읽기만 함 [https://jungle7-7610626261f4.herokuapp.com/pages/pintos-questions3.html](https://jungle7-7610626261f4.herokuapp.com/pages/pintos-questions3.html)  
- pintos3.pdf - 한 번 훑어봄 [https://drive.google.com/file/d/1k9uFXn-JzkAymGWq0ZU5PxTTxQoB_AHH/view?usp=sharing](https://drive.google.com/file/d/1k9uFXn-JzkAymGWq0ZU5PxTTxQoB_AHH/view?usp=sharing&authuser=0)  
- virtual memory - 한 번 읽어봤지만 정리가 안된듯 - [https://casys-kaist.github.io/pintos-kaist/project3/vm_management.html](https://casys-kaist.github.io/pintos-kaist/project3/vm_management.html)  
- anonymous page - 안 읽고 코드 돌진한 최후 [https://casys-kaist.github.io/pintos-kaist/project3/anon.html](https://casys-kaist.github.io/pintos-kaist/project3/anon.html)  
- stack growth - 아무것도 안 보고 여기까지 하느라 고생 좀 많았다 [https://casys-kaist.github.io/pintos-kaist/project3/stack_growth.html](https://casys-kaist.github.io/pintos-kaist/project3/stack_growth.html)

### 2023-10-10 발제

- 지난주에는...
	- stack argument passing을 했었지 function call을 흉내낸 것이다.
	- intel의 ring 구조, privileged inst를 사용하기 위해 시스템 콜을 만들었다
- 이번주에는...
	- virtual memory의 4-level tree구조 특별히 radix tree라고..!
	- SPT (Supplemental page table) : 페이지 테이블 말고 뭔가를 더 만들어야 한다. 가상메모리의 존재 이유를 다시 생각해보자.
		- isolation
		- **space sharing** ⟶ swap에 도움이 되는 정보
		- stack growth: 스택 전용 페이지를 따로 줘. 페이지가 터지면 다른 페이지 할당해서 붙여서 유지. 이젠 페이지가 스택용인지, text용인지 힙용인지
	- `mmap`, `munmap` 구현
	- 스와핑 구현

PintOS 취지 ⟶ debugging 하는 법을 배워가야 아이디어 구현에 도움이 될것.

### 2023-10-11

- Introduction 파트 읽고
	- [[supplemental page table 만들기 {pintos} {swjungle}]]
	- union 공부
	- page operations 생성 (struct page는 이미 있음.)
- before impl...
	- clone repo
	- [p] code review from Project 1 & 2
	- [p] live coding show

#### [[pintos3 {pdf} {pintos}]]

### 2023-10-12

- Introduction & Memory Management 파트 다시 읽는중
	- [[supplemental page table 만들기 {pintos} {swjungle}]]
	- `struct page_operations` 안에 있는 세 함수 포인터는 일종의 인터페이스로 활용된다. `VM_ANON`, `VM_UNINIT`, `VM_FILE` 종류에 따라 다른 구현체를 사용하도록 만든 케이스.
	- [x] 새로운 프로세스가 시작될 때 `__do_fork`에서 호출하는 `supplemental_page_table_init`을 구현하세요.
	- frame management
		- struct frame 안에 kva(kernel virtual addr)과 page(page 구조체 포인터)를 가지고 있다.
		- `palloc_get_page`에서 호출한다. 새로운 프레임을 가져오기 위해서 `frame` 정보를 활용한다. by `vm_get_frame(void) -> frame *` | swap out 구현은 뒤의 일이므로 해당 케이스에 대해서는 `PANIC("todo")` 이렇게 작성하래.
		- `vm_do_claim_page(page) -> bool` : 페이지와 프레임을 할당 (매핑?)해주는 함수. MMU가 보는 페이지 테이블의 정보를 수정해야 한다.
		- `vm_claim_page(va) -> bool`: va에 페이지를 할당하고 프레임을 할당해준다.
		- 페이지와 프레임을 분리한 덕분에 뒤에 나올 lazy loading이 가능해졌다.

- [[각종 QNA 정리 {swjungle}{pintos}{project3}]]

#### [[정글 대 토론회 {swjungle} {pintos}]]

### 2023-10-13

- [[lazy load segment {pintos}]]
- [[try handle fault + page claiming {pintos}]]
- [[virtual address, physical address, user pool, kernel pool {pintos}]]

### 2023-10-14

[[pintos3 {pdf} {pintos}]]  
[[Exploring Virtual Memory and Page Structures {blog}]]

### 2023-10-15

[[2023-10-15 pintos briefing {lazy_load_segment} {stack growth} {swjungle}]]
