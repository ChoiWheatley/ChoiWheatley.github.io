---
aliases: 
tags: 
description:
title: week 05 {swjungle} {malloc-lab}
created: 2023-09-07T22:10:12
updated: 2023-09-08T17:35:40
---
- [[0120 swjungle 🤖]]
- [[0121 CSAPP {swjungle}]]
	- WEEK04 못다 읽은 챕터들
		- [[3. Machine Level Representation of Programs]]
			- [[⭐️ 3.4 Accessing Information]]
			- [[⭐️ 3.7 Procedures]]
			- [[⭐️ 3.8 Array Allocation and Access]]
		- [[7. Linking]]
			- [[⭐️ 7.1. Compiler Drivers]]
			- [[⭐️ 7.4. Relocatable Object Files]]
			- [[⭐️ 7.9. Loading Executable Files]]
		- [[8. Exceptional Control Flow]]
			- 8.1
			- 8.5
		- [[9. Virtual Memory]]
			- **9.9**
			- 9.11
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
[[7. Linking]] 발표 @최승현

**09/11 월**  
시스템 콜 발표 @이인복

**09/12 화**  

**09/13 수**
