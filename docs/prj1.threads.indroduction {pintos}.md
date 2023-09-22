---
aliases: 
tags: 
description:
title: prj1.threads.indroduction {pintos}
created: 2023-09-22T16:46:46
updated: 2023-09-22T21:57:08
---
<https://casys-kaist.github.io/pintos-kaist/project1/introduction.html>
___

## dump

- synchronization primitives: {semaphors, lock, conditional vars, optimazation barriers}
- 더 이상 실행할 스레드가 없으면 (스레드 스케줄링?) 특수한 스레드인 `idle`을 실행시킨다.
- [[synchronization {pintos}]] 부터 읽고 스레드 기본 개념 이해
- [?] [page allocator](https://casys-kaist.github.io/pintos-kaist/appendix/memory_allocation.html#Page%20Allocator) 와 [block allocator](https://casys-kaist.github.io/pintos-kaist/appendix/memory_allocation.html#Block%20Allocator)와의 차이점?
- 아니, 인터럽트를 꺼서 race condition을 아예 원칙상 발생하지 않게 만들 수도 있대.
	- 극히 예외적인 케이스인 인터럽트 핸들러에서만 인터럽트를 끄고 사용한다고. `synch.c` 안에 있는 동기화 코드들도 인터럽트를 껐다고 가정한 채로 쓰여져 있다.
- `synch.c` => [Synchronization](https://casys-kaist.github.io/pintos-kaist/appendix/synchronization.html) 파트에서 다루는 다양한 개념인 semaphore, lock, condvar, optimization barriers 구현체. 
- `devices/`: 하드웨어 관리드라이버들. 키보드, 콘솔 출력, 인풋 레이어, 인터럽트 큐, 디스크 및 파티션, 시리얼 포트 드라이버, 블록 디바이스
- `kernel/list.c`, `kernel/list.h`  
    Doubly linked list implementation. Used all over the Pintos code, and you'll probably want to use it a few places yourself in project 1. **We recommand you to skim this code before you start (especially comments in the header file.)**

## file structure in `threads/`

- `loader.S`, `loader.h`: 어셈블리 코드. PC BIOS를 메모리에 적재하고 커널을 찾아 메모리에 적재한 후 `bootstrap()`으로 점프
- `kernel.lds.S`: 커널을 링크하기 위한 링커 스크립트. 
- `init.c/h`: 커널코드의 메인함수. 적어도 어떤 것들이 초기화 되는지는 알아야 함.
	- `bss_init`: `.bss` 영역 초기화?
	- `paging_init`: 페이지는 한 번에 읽는 메모리의 최소단위인데 뭘 초기화 한단거임?
	- 이 아래는 단순 파싱과 명령줄을 위한 코드들인듯
		- `read_command_line`
		- `parse_options`
		- `run_actions`
		- `usage`
	- `print_stats`: ???? 어떤 상태를 출력함?
	- **`main`**
		- bss 초기화, 명령줄 파싱, 스레드 초기화 콘솔 lock, 메모리
