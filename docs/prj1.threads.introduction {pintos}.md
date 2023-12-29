---
aliases: 
tags: 
description:
title: prj1.threads.introduction {pintos}
created: 2023-09-22T16:46:46
updated: 2023-09-25T13:56:32
---
- [[week07-10 {swjungle} {pintos}]]
- [pintos-kaist/project1/introduction (GH.io)](https://casys-kaist.github.io/pintos-kaist/project1/introduction.html)  
- [[synchronization {pintos} {semaphore} {lock} {monitor}]]
___

## dump

스레드가 생성될 때마다 스케줄에 컨텍스트가 하나씩 추가된다. `thread_create` 함수를 호출하며 넣은 함수 포인터는 스레드 단위의 main 함수가 되어 해당 함수를 벗어나면 스레드가 종료된다.

스케줄러는 스레드가 정확히 하나씩 실행할 수 있도록 스케줄링을 한다. 아무 스레드도 돌지 않는다면, 특수 스레드인 `idle` 스레드를 만들어 돌린다.

- [?] idle 스레드는 busy wait 하는가?

> "Idle 스레드"는 시스템이나 프로그램에서 사용되는 스레드 중 하나로, 주로 다른 스레드들이 작업을 수행하지 않는 시간에 시스템 리소스(예: CPU 시간)를 활용하거나 관리하기 위해 사용됩니다. 이 스레드는 일반적으로 아무런 작업을 하지 않고 대기 상태에 들어가며, CPU 자원을 점유하지 않습니다. 따라서 busy wait와는 대조적으로 CPU를 무한히 점유하지 않습니다.

> 일반적인 운영체제에서 idle 스레드는 CPU 스케줄링을 조절하고 전력 관리를 위해 사용됩니다. 예를 들어, 다른 스레드가 실행 중이 아니라면 CPU가 idle 스레드에게 할당되어 전력을 절약하거나 리소스 관리를 수행할 수 있습니다. 이는 시스템의 성능을 최적화하고 전력 소비를 관리하는 데 도움이 됩니다.

활성상태를 유지하면서 전력소비를 줄이기 위한 깡통 스레드.
___

스레드, 컨텍스트는 1대1로 존재하며, 스케줄러가 강제적으로 한 스레드에서 다른 스레드로 문맥전환(context-switch) 하기 위해 기존 스레드의 컨텍스트를 저장하고 다음 스레드를 복원하는 작업을 수행하여야 한다. 이 작업을 [thread_launch](../threads/thread.c#thread_launch) 함수에서 수행한다.

GDB를 사용하면서 컨텍스트 스위칭이 일어나는 타이밍을 확인할 수도 있다. 대표적으로 [`do_iret()`](../threads/thread.c#do_iret) 함수에서 `iret`을 실행할 때 어떻게 되는지 확인해보자.

기본적으로, PintOS는 새 스레드를 만들때 고정 크기의 실행스택이 할당된다. 따라서 유저 프로그램이 너무 큰 지역변수를 스레드 안에서 선언해버리면 커널패닉이 발생할 수도 있다. 대안으로 블록/페이지 할당자가 있다고. ([Memory Allocation](https://casys-kaist.github.io/pintos-kaist/appendix/memory_allocation.html))

- synchronization primitives: {semaphors, lock, conditional vars, optimazation barriers}
- 더 이상 실행할 스레드가 없으면 (스레드 스케줄링?) 특수한 스레드인 `idle`을 실행시킨다.
- [[synchronization {pintos} {semaphore} {lock} {monitor}]] 부터 읽고 스레드 기본 개념 이해
- [?] [page allocator](https://casys-kaist.github.io/pintos-kaist/appendix/memory_allocation.html#Page%20Allocator) 와 [block allocator](https://casys-kaist.github.io/pintos-kaist/appendix/memory_allocation.html#Block%20Allocator)와의 차이점?
- 아니, 인터럽트를 꺼서 race condition을 아예 원칙상 발생하지 않게 만들 수도 있대.
	- 극히 예외적인 케이스인 인터럽트 핸들러에서만 인터럽트를 끄고 사용한다고. `synch.c` 안에 있는 동기화 코드들은 인터럽트를 끈 상태로 
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
- `synch.c` => [[synchronization {pintos} {semaphore} {lock} {monitor}]] 파트에서 다루는 다양한 개념인 semaphore, lock, condvar, optimization barriers 구현체. 
