---
aliases: 
tags: 
description: threads
title: week07-09 {swjungle} {pintos}
created: 2023-09-21T16:20:48
updated: 2023-09-21T20:43:38
---
- [[0120 swjungle 🤖]]
- [[0121 CSAPP Third Edition Bryant, Randal E. O'Hallaron, David.|csapp]]
	- [[3. Machine Level Representation of Programs]]
	- [[8. Exceptional Control Flow]]
	- [[12. Concurrent Programming]]
- [kaist pintos assignment specification](https://casys-kaist.github.io/pintos-kaist/)
	- [kaist pintos 과제 설명서 한글 번역본](https://yjohdev.notion.site/KAIST-PINTOS-ebdc8be9d02d4475a4675c7b920e3653)
- 
___

## README

7-9주차는 pintos 주간. 어떻게든 테스트 코드 all pass에 도달하며 지식을 쌓기위한 주간이다. 만 줄이 넘어가는 코드를 분석하는 경험은 토이 프로젝트만 반복하던 주니어 개발자에겐 챌린지겠지만 일반적인 부트캠프가 줄 수 있는 지식과는 분명히 다른 차원의 지혜를 얻어갈 것이라고 기대한다. 

7주차는 추석연휴로 인해 1.5주로 연장되었다. 따라서 7주차는 10월 3일 화요일이 마감이다. 그 이후로 모든 주차는 주의 마감이 화요일이 된다는 점도 유의하자.

이번 주차의 팀은 박세준, 조가람, 최승현 이렇게 세 명이다. 오전 10시에 모여 화이팅하고 그날 저녁(시간미정)에 모여 브리핑을 할 것이다. 학습기간 동안에는 공부한 것들에 대한 내용을 다루고 구현기간 동안에는 다 함께 머리를 맞댄 결과물에 대해서 이야기를 나누게 될 것 같다.

추석기간에 빠지는 날이 팀원들이 서로 다르다. 
- 박세준: 2023-09-27(저녁) ~ 2023-09-30까지
- 조가람: 2023-09-30 ~ 2023-10-01 빠진다. 
- 최승현: 하루 날잡고 등산할 예정.

권영진 교수님의 OS 강의일정
- 2023-09-26T10:30:00
- 2023-10-10T10:30:00
- OS abstraction 개념에 초점을 맞추어 진행.
- 강의 슬라이드는 swjungle 페이지에서.
- [CS 6200: Introduction to Operating Systems Course Videos (Georgia Tech College of Computing)](https://omscs.gatech.edu/cs-6200-introduction-operating-systems-course-videos)

## dump

- pintos prj01 \<thread\> 설명 <https://youtu.be/myO2bs5LMak?si=8SmqdzUOKnTZO2dc>
