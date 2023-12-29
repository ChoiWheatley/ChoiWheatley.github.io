---
aliases: 
tags: 
description:
title: week 05 {swjungle} {malloc-lab}
created: 2023-09-07T22:10:12
updated: 2023-09-14T19:59:26
---
- [[0120 swjungle 🤖]]
- [[0121 CSAPP Third Edition Bryant, Randal E. O'Hallaron, David.]]
	- WEEK04 못다 읽은 챕터들
		- [[3. Machine Level Representation of Programs {CSAPP}]]
			- [[⭐️ 3.4 Accessing Information]]
			- [[⭐️ 3.7 Procedures]]
			- [[⭐️ 3.8 Array Allocation and Access]]
		- [[7. Linking {CSAPP}]]
			- [[⭐️ 7.1. Compiler Drivers]]
			- [[⭐️ 7.4. Relocatable Object Files (ELF File Format)]]
			- [[⭐️ 7.9. Loading Executable Files]]
		- [[8. Exceptional Control Flow]]
			- [[⭐️ 8.1 Exceptions]]
			- [[⭐️ 8.5. Signals]]
		- [[9. Virtual Memory]]
			- [[⭐️ 9.9. Dynamic Memory Allocation]]
			- [[⭐️ 9.11. Common Memory-Related Bugs in C Programs]]
	- WEEK05 읽어두면 좋을 챕터들 
		- 9.9장, 동적 메모리 할당부터
		- 6장, 메모리 계층구조
		- 7장, 예외적인 제어흐름
		- 8장, 가상메모리
- [ChoiWheatley/malloclab-jungle]
- [malloclab 과제 설명서 {pdf}](http://csapp.cs.cmu.edu/3e/malloclab.pdf)
- <http://csapp.cs.cmu.edu/3e/labs.html> → CSAPP에서 마련된 각종 과제들 모음집
- [cmu_system_programming.zip {Google Drive}](https://drive.google.com/file/d/1T2kKzfeRTvhYphzLZ0W3hUFf-YJpt5wV/view?pli=1)
	- 관련 챕터
	    - malloc lab과 직접 관련되는 section들
	        - 19장: malloc-basic
	            - GNU malloc API와 다양한 구현 방식 설명
	            - implicit list 방식의 malloc 구현에 관한 자세한 설명
	        - 20장: malloc-advanced
	            - explicit & seg list방식의 malloc 구현, garbage collection 개념에 대한 설명
	            - well-known memory bug에 관한 설명
	            - 44p : gdb, valgrind등의 memory bug를 잡기위한 접근 방식 설명 포함
	    - malloc lab 수행에 도움을 줄 수 있는 내용을 담은 section들
	        - 14장: ecf-procs
	            - exception의 개념과 종류, process의 개념에 대한 설명
	            - 14p : segmentation fault에 관한 설명 포함
	        - 11장: memory-hierarchy
	            - 컴퓨터 시스템의 메모리 계층구조에 대한 설명
	        - 17장: vm-concepts
	            - 가상 메모리와 page fault에 대한 이론적 설명
- WEEK05 키워드
	- 메모리 단편화
	- 메모리 할당 정책
	- 시스템 콜
	- sbrk
	- mmap
___

## README

팀원들과 전략을 세웠다. CSAPP 책을 따라 읽는것이 일주일의 80%가 될 것이라고, 따라서 챕터를 다 같이 공부하되, 발표하고 싶은 챕터를 따로 더 공부하여 매일매일 발표하는 식으로 진행될 것 같다. 일단 금요일은 포괄적인 내용을 다룰 예정. 무엇무엇이 있는지, 어떤 챕터를 중점적으로 읽을 것인지, 지식을 팀원들과 싱크 맞추는 것이다.

## Daily Dump

**09/07 목**  ~ **09/08 금**  
밀린 책 부분 읽고 발표 준비 하기

**09/09 토**  
[[⭐️ 9.9. Dynamic Memory Allocation]] 발표 @안상언

**09/10 일**  
[[7. Linking {CSAPP}]] 발표 @최승현

**09/11 월**  
시스템 콜 발표 @이인복

**09/12 화**  

**09/13 수**

## [[malloclab]]

## syscall @이인복

user mode by programmer VERSUS previledge mode by kernel

중간에 이벤트가 발생해서 인터럽트 or 시스템 콜 발생했을 때 커널코드쪽으로 실행 흐름이 옮겨갔다가 다시 돌아옴.

CPU의 상태를 저장. 나중에 실행을 이어가야 하니까. 

### WHY syscall?

부팅하면 BIOS 화면 나타나고, Power self test? 같은게 나타난다. 정상인지 확인하고 보조기억장치 (DISK, ROM)에 접근한다. 

> OS또한 프로그램이고 RAM에 올라가야 실행이 된다.

interface between Kernel and Programmer, actually not that directly, proxy such as shell, stdio libraries

### Kind of syscalls

**Interrupt**

- hardware level (=Interrupt)  
	power, io, timer, 
- program level (=Trap)  
	div by 0, unauthorized memory access

**Process/Threads**

**File IO**

**Socket**

**Device**

**IPC**

### state machine diagram

![[스크린샷 2023-09-11 오후 5.21.38.png|500]]

## Quiz

### 5. 

> 다음과 같은 함수를 실행하면 어떤 문제가 생기는지 논하시오. (msg 로 전달되는 문자열의 길이가 64 이상인 경우를 가정하시오.)  이와 유사한 코드를 포함한 소프트웨어는 어떤 문제가 발생할 수 있을까?  아래 코드를 어떻게 수정하면 문제가 발생하지 않을 수 있을까?  

```c
void print_err(char *msg) {  
	char *s = (char *) malloc(10);  
	sprintf(s, "%s\n", msg);  
	fprintf(stderr, s);  
	free(s);  
};
```

**문제의 요지**  
malloc을 구현해봤기 때문에 malloc이 지정해준 영역보다 넘치는 공간을 사용했을 시 어떤 영향이 가해지는지 알 수 있다.

**정답**  
페이로드 밖의 영역에 있는 footer, 다음 블럭의 header가 깨진다. malloc 관리정보가 파괴되므로 추후 malloc/free 호출 시 엉뚱한 주소를 참조할 수 있으며, 나중에 디버깅도 어려워진다.

**모범코드**

```c
char *s = (char *)malloc(strlen(msg) + 2);
```

### extra A.

> 메모리를 할당하는데 왜 블럭단위로 할당하는지 설명해 보시오.

- 임의 크기로 할당하다보면 외부 단편화가 많이 발생한다. 블록 단위로 하면 외부 단편화를 줄일 수 있다. 
- 모든 primitive 자료형을 담을 수 있는 최소한의 크기를 블럭으로 지정해 놓음으로써 일관성있게 메모리를 사용할 수 있게 하기 위해서.
