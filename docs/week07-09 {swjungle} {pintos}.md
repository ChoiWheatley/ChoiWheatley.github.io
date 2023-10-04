---
aliases: 
tags: 
description: threads
title: week07-09 {swjungle} {pintos}
created: 2023-09-21T16:20:48
updated: 2023-10-04T23:55:24
---
- [[0120 swjungle 🤖]]
- [[0121 CSAPP Third Edition Bryant, Randal E. O'Hallaron, David.|csapp]]
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

이번 주차의 팀은 박세준, 조가람, 최승현 이렇게 세 명이다. 오전 10시에 모여 화이팅하고 그날 저녁(시간미정)에 모여 브리핑을 할 것이다. 학습기간 동안에는 공부한 것들에 대한 내용을 다루고 구현기간 동안에는 다 함께 머리를 맞댄 결과물에 대해서 이야기를 나누게 될 것 같다.

추석기간에 빠지는 날이 팀원들이 서로 다르다. 
- 박세준: 2023-09-27(저녁) ~ 2023-09-30까지
- 조가람: 2023-09-30 ~ 2023-10-01 빠진다. 
- 최승현: 하루 날잡고 등산할 예정.

## 권영진 교수님의 OS 강의

- ~~2023-09-26T10:30:00~~
	- [[2023-09-26 권영진 교수님의 OS 강의 (1차) {swjungle}]]
- 2023-10-10T10:30:00
- OS abstraction 개념에 초점을 맞추어 진행.
- [Pintos_1.pdf](https://drive.google.com/file/d/1rr1VobnaR8QiWq3TVImvzzHWWdB5d4B5/view)
- [01_os_review.pdf](https://drive.google.com/file/d/1v7ZT0uCqnSFQQY3jQsnXnCh9WHPpgQxZ/view)
- [CS 6200: Introduction to Operating Systems Course Videos (Georgia Tech College of Computing)](https://omscs.gatech.edu/cs-6200-introduction-operating-systems-course-videos)

## week07 - Threads

**INDEX**

- [Pintos Project1-1 Thread by Yujip Won {YT}](https://github.com/ChoiWheatley/swjungle-week07-09/blob/master/doc/Project1%20Threads.md)
- `pintos -- -q`: 실행 할 거 다 하면 자동으로 종료 (Ctrl+C 한다고 안꺼짐)

**2023-09-23**

- [[2023-09-23 dump {swjungle} {priority-scheduling} {alarm-clock}]]


**2023-09-28**

- [[2023-09-28 dump {swjungle}]]

**2023-09-29**

- [[priority-donate-multiple {swjungle} {pintos}]]
- [[priority-donate-nest, chain {swjungle} {pintos}]]

**2023-10-01 ~ 2023-10-02**

- [[Multi Level Feedback Queue {swjungle}{pintos}]]
- [[week07 WIL 정리, 발표준비 {swjungle}]]
	- [Project1 Threads.md {GH}](https://github.com/ChoiWheatley/swjungle-week07-09/blob/master/doc/Project1%20Threads.md)

**2023-10-03**

- [[priority-donate-sema {swjungle}{pintos}]]
- [[priority inversion on lock release {swjungle}{pintos}]]

## week08 - User Program

### INDEX

- csapp
	- [[3. Machine Level Representation of Programs {CSAPP}]]
	- [[8. Exceptional Control Flow]]  | 8.2, 8.3, 8.4
- faq
	- [pintos-kaist/project2/FAQ.html](https://casys-kaist.github.io/pintos-kaist/project2/FAQ.html)
	- [지난 기수의 질문 아카이브](https://jungle7-7610626261f4.herokuapp.com/pages/pintos-questions2.html)
	- [[각종 QNA 정리 {swjungle}{pintos}{project2}]]
- pintos-kaist
	- [gitbook :: Project 2 Introduction](https://casys-kaist.github.io/pintos-kaist/project2/introduction.html)
	- [gitbook :: Appendix :: Virtual Address](https://casys-kaist.github.io/pintos-kaist/appendix/virtual_address.html)
	- [gitbook 한글번역 {Notion}](https://www.notion.so/PROJECT-2-USER-PROGRAM-b019874b02f645d7813c554bd7377770)

### 2023-10-03 발제 아카이브

- gpt, bing chat, bard, 클로바 x 등등 가리지 말고 다 써봐라. 생성형 AI가 언제나 정답을 이야기해주지는 않으니.
- TDD spec이라고 불리우는 엔티티는 specification의 약자로, "이렇게 이렇게 돌아갔으면 좋겠어요" 요구사항을 명세로 작성할 때부터 이미 테스트케이스로 만들어버린다. 그리고 테스트 코드를 통과하도록 코드를 작성한다.
	- TDD가 좋은 점은 컴퓨터에게 단순 노가다를 맡길 수 있다는 점이다. TDD를 맹신하면 안되는게, 테스트 코드 통과에만 목을 매다는 경우가 발생할 수 있기 때문이다.
- week07 Thread을 구현하면서 느낀점 - Timer HW가 없으면 스레드도 없다. ⟶ preemptive scheduling
- [x] [[Monitor의 condvar가 하나일 필요가 없나?]]
- PRJ2 질문목록을 옆에 끼고, GPT와 여러 생성형 AI를 옆에 끼고 살자. 내가 할 질문은 이미 누군가가 했을수도 있다.
- [[3. Machine Level Representation of Programs {CSAPP}]]에서 나오는 레지스터들과 기타 하드웨어 알아둘 지식들 [[Hardware Knowledges for PintOS {swjungle}]]
- [[8. Exceptional Control Flow]]
	- 8.2. Processes
	- 8.3. System Call Error Handling
	- 8.4. Process Control
- Argument Passing
	- 기본적인 function call에서조차 단순 `call` 과 `ret` 만으로 함수 호출 / 반환이 이루어지지 않는다. 반드시 레지스터의 도움을 필요로 한다.
	- 실행파일이 인자와 함께 메모리에 올라가는 과정에 대한 이해가 필요할 것이다.
- Process
	- Abstraction of Machine
	- Protection --- HW의 도움을 받아야 해.
	- system call --- 총 14개나 되는데, 그걸 다 구현해야 해. 근데 그게 다가 아니야. arg passing, user mem access를 다루고 난 뒤에야 진행할 수 있음
	- Process가 끝날 때엔 무슨 일이 일어나지?
	- fd 복제 with [[dup2(2)]]

![[Pasted image 20231003145914.png]]

### 2023-10-04

- [[각종 QNA 정리 {swjungle}{pintos}{project2}]]
- limitation from simple file system
	- 파일 시스템 코드를 사용하는 동안 internal synchronization을 사용하지 마세요. Project4가 진행되기 전까지는 한 번에 하나의 프로세스가 파일 시스템 코드를 실행한다는 것을 보장하는 용도로만 synchronization을 사용하세요.

- **argument passing**
	- `process_exec()` 함수는 새로운 프로세스에 인자를 전달하는 것을 지원하지 않음. 따라서 이를 확장구현하여 공백을 기준으로 여러 단어로 나누어지게 만들어라.
