---
aliases: 
tags: 
description:
title: week06 {swjungle}{proxy-lab}
created: 2023-09-14T13:34:26
updated: 2023-09-17T22:48:13
---
- [[0120 swjungle 🤖]]
- [[0121 CSAPP Third Edition Bryant, Randal E. O'Hallaron, David.]]
- [[10. System-Level IO {CSAPP}]]
- [[11. Network Programming {CSAPP}]]
- [[Socket Programming C API]]
- [[Computer Networking, a top down approach - Kurose, James F. Ross, Keith W.]]
- [10]
___

## README

6주차 접어들면서 실패와 좌절감을 겪은 사람들이 많아지고 있다. 한 주제에 매몰되어 시간을 낭비하기보다는 잠깐 다른 일을 하다가 돌아와 신선한 마음으로 다시 도전하는 자세와 같이 다양한 전략들이 나왔다. 이번 주차에서는 시간을 낭비하지 않고, 머리를 쥐어뜯지 않고 행복하게, 그러나 확실하게 학습하는 주간이 되었으면 한다.

**진행방법**

1. 책에 있는 코드를 기반으로, tiny 웹서버를 완성하기 (tiny/tiny.c, tiny/cgi-bin/adder.c 완성)
2. AWS 혹은 container 사용시 외부로 포트 여는 것을 잊지 말기
3. 숙제 문제 풀기 (11.6c, 7, 9, 10, 11)
4. 프록시 과제 도전 (proxy.c 완성)

**GOAL**

- tiny 웹서버를 만들고 → 숙제문제 11.6c, 7, 9, 10, 11 중 세문제 이상 풀기 → 프록시 과제 도전
    - 브라우저의 발달에 따라 문제 11.7은 _MPG 파일이 아니고 **MP4 파일**을 처리하도록 문제를 변경

## Daily Cheer Up

이번 주차는 매일 두 번 만나게 되는데, 아침 **09:30**에 모여 모닝 파이팅을 외치고 저녁 **20:00**에 모여 지친 정신을 한 번 일깨우는 시간을 가지게 될 것이다. 

### 2023-09-14 목

**TLDR**

- 09-15 금요일까지 `tiny/tiny.c`, `tiny/cgi-bin/adder.c` 완성
- 09-17 일요일까지 건너뛰었던 숙제 문제 11.6c, 11.7, 11.9, 11.10, 11.11 모두 풀면서 놓친 지식 줏어담기
- 09-18 월요일부터 [[proxylab]] 구현 시작하기, 구체적인 것들은 일요일 브리핑 시간 전까지 조사 후 결정

6주차 전략 수립, 어떻게 공부해왔는지, 바뀐 공부전략에 대해서 이야기함. 대체로 비슷하다. 

> 책에 매몰되지 말자

인터넷에 검증되지 않은 자료를 배척하기보다는 책을 활용하여 스택오버플로나 블로그 글을 교차검증하며 빠르게 이해해보자. 책만 주구장창 읽기보다는 인덱싱과 검색용으로, 얼마든지 다시 돌아올 수 있게 만드는 것이 더 바람직하다.

> 남이 쓴 C 코드 보는 것을 부끄러워 하지 말라

C 코드 보고 이해하는 것만으로도 얻어가는 구조, 지식이 많다. 우리가 밖에 나가서 C로 프로젝트를 할 일은 없을 테니까 그냥 일단 봐라!

### 2023-09-15 금

**내일 오후 8시까지**

- 동영상 강의 시청(2개) : 가벼운 퀴즈를 준비해오기. 진짜 궁금한것들 정리해오기.
- osi 7 계층 : 이것저것 공부하기
- 책읽기 준비 : 10장(가볍게) 11장(딥하게) 11장에 있는 구현문제를 제외한 간단한 문제만 풀어보기

**일요일**

- 연습문제를 몰아서 풀기!!! (안되면 월요일 오전)

월요일부터 프록시 시작하기

### 2023-09-16 토

- 내일 오후 8시까지 연습문제를 다 풀어보자.
- 구현문제는 가능한 많이

### 2023-09-17 일

- 월요일 15:00까지 숙제 문제 적어도 3 문제씩 풀고 풀이경험 공유하기
- 프록시 랩 발제

### 2023-09-18 월



### 2023-09-19 화

### 2023-09-20 수

## CSAPP

[[0121 CSAPP Third Edition Bryant, Randal E. O'Hallaron, David.]]

- 이번주 필수
	- [[11. Network Programming {CSAPP}]]
- 읽어두면 좋은 챕터들
	- [[8. Exceptional Control Flow]]
		- [[⭐️ 8.1 Exceptions]]
		- [[⭐️ 8.5. Signals]]
	- [[10. System-Level IO {CSAPP}]]
		- 10.5 Robust Reading and Writing with the RIO Package | 과제할 때 필요한 `rio_read`, `rio_write` 접두어 함수들
	- [[12. Concurrent Programming {csapp}]]
- 지난주 못 다 읽은 챕터들
	- [[9. Virtual Memory]] 
		- 초반부 Physical and Virtual Addressing, VM as a Tool for blah blah, address translation **Summary**
		- [[⭐️ 9.9. Dynamic Memory Allocation]] 중 Segregated free list 중 **Buddy System**
		- 9.10  garbage collection
		- [[⭐️ 9.11. Common Memory-Related Bugs in C Programs]]
