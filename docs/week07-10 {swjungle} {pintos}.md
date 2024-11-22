---
aliases: 
tags: 
description: threads
title: week07-10 {swjungle} {pintos}
created: 2023-09-21T16:20:48
updated: 2024-01-05T00:09:09
---
- [[swjungle 🤖]]
- weekly
	- [[week07 - Threads {pintos} {swjungle}]]
	- [[week08 - User Program {pintos} {swjungle}]]
	- [[week09 - Virtual Memory {pintos} {swjungle}]]
- [[0015.1 CSAPP Third Edition Bryant, Randal E. O'Hallaron, David. 💻|csapp]]
	- [[3. Machine Level Representation of Programs {CSAPP}]]
		- [[Hardware Knowledges for PintOS {swjungle}]]
	- [[8. Exceptional Control Flow]]
	- [[12. Concurrent Programming {csapp}]]
- [[kaist pintos assignment specification {casys-kaist.github.io}]]
	- [kaist pintos 과제 설명서 한글 번역본](https://yjohdev.notion.site/KAIST-PINTOS-ebdc8be9d02d4475a4675c7b920e3653)
	- [pintos slides by Yujip Won](https://oslab.kaist.ac.kr/pintosslides/)
- [stanford pintos.pdf](https://web.stanford.edu/class/cs140/projects/pintos/pintos.pdf)
- [swjungle-week07-09 {GH}](https://github.com/ChoiWheatley/swjungle-week07-09) | 학습관련 정리를 이번엔 깃허브 마크다운으로 공용으로 관리해볼 예정.
- [[Hardware Knowledges for PintOS {swjungle}]]
- [[Operating Systems Three Easy Pieces]]
___

## README

7-9주차는 pintos 주간. 어떻게든 테스트 코드 all pass에 도달하며 지식을 쌓기위한 주간이다. 만 줄이 넘어가는 코드를 분석하는 경험은 토이 프로젝트만 반복하던 주니어 개발자에겐 챌린지겠지만 일반적인 부트캠프가 줄 수 있는 지식과는 분명히 다른 차원의 지혜를 얻어갈 것이라고 기대한다. 

7주차는 추석연휴로 인해 1.5주로 연장되었다. 따라서 7주차는 10월 3일 화요일이 마감이다. 그 이후로 모든 주차는 주의 마감이 화요일이 된다는 점도 유의하자.

## 권영진 교수님의 OS 강의

- ~~2023-09-26T10:30:00~~
	- [[2023-09-26 권영진 교수님의 OS 강의 (1차) {swjungle}]]
- 2023-10-10T10:30:00
	- [[2023-10-10 권영진 교수님 OS 강의 (2차) {swjungle}]]
- OS abstraction 개념에 초점을 맞추어 진행.
- [Pintos_1.pdf](https://drive.google.com/file/d/1rr1VobnaR8QiWq3TVImvzzHWWdB5d4B5/view)
- [01_os_review.pdf](https://drive.google.com/file/d/1v7ZT0uCqnSFQQY3jQsnXnCh9WHPpgQxZ/view)
- [CS 6200: Introduction to Operating Systems Course Videos (Georgia Tech College of Computing)](https://omscs.gatech.edu/cs-6200-introduction-operating-systems-course-videos)

## READ

- 병철 추천 - [https://blog.xenoscr.net/2021/09/06/Exploring-Virtual-Memory-and-Page-Structures.html](https://blog.xenoscr.net/2021/09/06/Exploring-Virtual-Memory-and-Page-Structures.html)
- x86-64 isa - 한 번 훑어봄. 가상메모리와 pml4 포스팅도 확인바람. [https://it-eldorado.tistory.com/35](https://it-eldorado.tistory.com/35)  
- [eldorado.tistory.com - 가상 메모리](https://it-eldorado.tistory.com/52)
- 지난기수 질문답변 - load segment 위주로 읽기만 함 [https://jungle7-7610626261f4.herokuapp.com/pages/pintos-questions3.html](https://jungle7-7610626261f4.herokuapp.com/pages/pintos-questions3.html)  
- pintos3.pdf - 한 번 훑어봄 [https://drive.google.com/file/d/1k9uFXn-JzkAymGWq0ZU5PxTTxQoB_AHH/view?usp=sharing](https://drive.google.com/file/d/1k9uFXn-JzkAymGWq0ZU5PxTTxQoB_AHH/view?usp=sharing&authuser=0)  
- virtual memory - 한 번 읽어봤지만 정리가 안된듯 - [https://casys-kaist.github.io/pintos-kaist/project3/vm_management.html](https://casys-kaist.github.io/pintos-kaist/project3/vm_management.html)  
- anonymous page - 안 읽고 코드 돌진한 최후 [https://casys-kaist.github.io/pintos-kaist/project3/anon.html](https://casys-kaist.github.io/pintos-kaist/project3/anon.html)  
- stack growth - 아무것도 안 보고 여기까지 하느라 고생 좀 많았다 [https://casys-kaist.github.io/pintos-kaist/project3/stack_growth.html](https://casys-kaist.github.io/pintos-kaist/project3/stack_growth.html)

## [[week07 - Threads {pintos} {swjungle}]]

- [Pintos Project1-1 Thread by Yujip Won {YT}](https://github.com/ChoiWheatley/swjungle-week07-09/blob/master/doc/Project1%20Threads.md)에 대한 내용이 포함되어 있습니다.
- `pintos -- -q` 명령어가 프로그램 실행을 모두 마치면 자동으로 종료되는 기능에 대한 정보가 있습니다.
- 2023-09-23:
    - [swjungle] 및 [priority-scheduling], [alarm-clock] 관련 정보가 포함되어 있습니다.
- 2023-09-28:
    - dump관련된 내용이 포함되어 있습니다.
- 2023-09-29:
    - [priority-donate-multiple], [priority-donate-nest], [chain]에 대한 내용이 포함되어 있습니다.
- 2023-10-01 ~ 2023-10-02:
    - [Multi Level Feedback Queue]와 [week07 WIL 정리, 발표준비]에 대한 정보가 있습니다.
- 2023-10-03:
    - [priority-donate-sema] 및 [priority inversion on lock release]에 대한 내용이 있습니다.

## [[week08 - User Program {pintos} {swjungle}]]

- csapp:
    - 레지스터 및 기타 하드웨어에 대한 지식이 포함되어 있습니다.
    - 프로세스, 시스템 호출 오류 처리, 프로세스 제어에 대한 설명이 있습니다.
- faq:
    - Pintos 프로젝트에 대한 FAQ 목록이 포함되어 있습니다.
- pintos-kaist:
    - 프로젝트 2 소개에 대한 설명이 있습니다.
    - 가상 주소에 대한 추가 설명이 있습니다.
    - 한글 번역 가이드가 제공됩니다.
- 2023-10-03 발제 아카이브:
    - TDD(Test Driven Development)에 대한 개념과 주의사항에 대해 언급되었습니다.
    - 스레드 및 프로세스에 대한 경험과 통제에 대한 내용이 포함되어 있습니다.
- 2023-10-04:
    - 파일 시스템 코드 사용 시 동기화 제한 사항이 언급되었습니다.
    - `process_exec()` 함수의 인자 전달에 대한 내용이 있습니다.
- 2023-10-05:
    - 단일 테스트 실행 방법에 대한 정보가 제공됩니다.
- 2023-10-06:
    - 자주 보이는 주솟값과 syscall 관련 정보가 있습니다.
    - page_fault() 함수와 관련된 내용이 포함되어 있습니다.
- Weekly I learned:
    - 사용자 프로그램에 대한 프로젝트 2에 대한 내용이 포함되어 있습니다.

## [[week09 - Virtual Memory {pintos} {swjungle}]]

- [[각종 QNA 정리 {swjungle}{pintos}{project3}]]에서는 PintOS 프로젝트 3과 관련된 질문과 답변이 포함되어 있습니다.
- [[pintos3 {pdf} {pintos}]]에서는 Top-Down 접근 방식으로 PintOS에 대한 자세한 내용이 담겨 있습니다.
- pintos-kaist gitbook:
    - Introduction과 FAQ 섹션에 대한 정보가 있습니다.
    - 한글 번역 가이드에 대한 링크가 포함되어 있습니다.
- CSAPP의 [[9. Virtual Memory {CSAPP}]]에서 가상 메모리에 대한 내용이 포함되어 있습니다.
- Yujip Won의 슬라이드에서는 가상 메모리와 관련된 두 개의 PDF 파일에 대한 정보가 있습니다.
- 다양한 외부 링크들이 포함되어 있습니다. 이 링크들은 가상 메모리 및 페이지 구조에 대한 자세한 내용을 포함하고 있습니다.
- 2023-10-10 발제에서는 이번주와 지난주에 다룬 내용과 PintOS의 취지에 대한 정보가 포함되어 있습니다.
- 2023-10-11에는 PintOS의 자료구조와 관련된 내용과 관련하여 클론 레포 및 코드 리뷰에 대한 정보가 있습니다.
- 2023-10-12에는 PintOS의 메모리 관리와 프레임 관리에 대한 정보가 포함되어 있습니다.
- 2023-10-13에는 lazy load 세그먼트, 페이지 처리 및 가상 주소, 물리 주소, 사용자 풀, 커널 풀에 대한 내용이 있습니다.
- 2023-10-14에는 PintOS 프로젝트에 대한 PDF 자료와 가상 메모리 및 페이지 구조를 탐색하는 블로그 게시물에 대한 정보가 있습니다.
- 2023-10-15에는 PintOS의 lazy load 세그먼트와 스택 성장에 관한 브리핑이 포함되어 있습니다. [[2023-10-15 pintos briefing {lazy_load_segment} {stack growth} {swjungle}]]
