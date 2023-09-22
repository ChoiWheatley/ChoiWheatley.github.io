---
aliases: 
tags: 
description:
title: prj1.threads.indroduction {pintos}
created: 2023-09-22T16:46:46
updated: 2023-09-22T17:36:29
---
<https://casys-kaist.github.io/pintos-kaist/project1/introduction.html>
___

## dump

- synchronization primitives: {semaphors, lock, conditional vars, optimazation barriers}
- 더 이상 실행할 스레드가 없으면 (스레드 스케줄링?) 특수한 스레드인 `idle`을 실행시킨다.
- [[synchronization {pintos}]] 부터 읽고 스레드 기본 개념 이해

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
