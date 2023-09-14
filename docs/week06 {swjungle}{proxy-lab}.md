---
aliases: 
tags: 
description:
title: week06 {swjungle}{proxy-lab}
created: 2023-09-14T13:34:26
updated: 2023-09-14T14:35:10
---
- [[0120 swjungle 🤖]]
- [[0121 CSAPP {swjungle}]]
	- 이번주 필수
		- [[11. Network Programming]]
	- 읽어두면 좋은 챕터들
		- [[8. Exceptional Control Flow]]
			- [[⭐️ 8.1 Exceptions]]
			- [[⭐️ 8.5. Signals]]
		- [[10. System-Level IO]]
			- 10.5 Robust Reading and Writing with the RIO Package
		- [[12. Concurrent Programming]]
	- 지난주 못 다 읽은 챕터들
		- [[9. Virtual Memory]] 
			- 초반부 Physical and Virtual Addressing, VM as a Tool for blah blah, address translation **Summary**
			- [[⭐️ 9.9. Dynamic Memory Allocation]] 중 Segregated free list 중 **Buddy System**
			- 9.10  garbage collection
			- [[⭐️ 9.11. Common Memory-Related Bugs in C Programs]]
___

## README

6주차 접어들면서 실패와 좌절감을 겪은 사람들이 많아지고 있다. 한 주제에 매몰되어 시간을 낭비하기보다는 잠깐 다른 일을 하다가 돌아와 신선한 마음으로 다시 도전하는 자세와 같이 다양한 전략들이 나왔다. 이번 주차에서는 시간을 낭비하지 않고, 머리를 쥐어뜯지 않고 행복하게, 그러나 확실하게 학습하는 주간이 되었으면 한다.

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
