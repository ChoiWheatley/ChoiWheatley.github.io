---
aliases: 
tags: 
description: threads
title: week07-09 {swjungle} {pintos}
created: 2023-09-21T16:20:48
updated: 2023-10-02T19:09:04
---
- [[0120 swjungle 🤖]]
- [[0121 CSAPP Third Edition Bryant, Randal E. O'Hallaron, David.|csapp]]
	- [[3. Machine Level Representation of Programs {CSAPP}]]
	- [[8. Exceptional Control Flow]]
	- [[12. Concurrent Programming {csapp}]]
- [[kaist pintos assignment specification {casys-kaist.github.io}]]
	- [kaist pintos 과제 설명서 한글 번역본](https://yjohdev.notion.site/KAIST-PINTOS-ebdc8be9d02d4475a4675c7b920e3653)
- [swjungle-week07-09 {GH}](https://github.com/ChoiWheatley/swjungle-week07-09) | 학습관련 정리를 이번엔 깃허브 마크다운으로 공용으로 관리해볼 예정.
___

## README

7-9주차는 pintos 주간. 어떻게든 테스트 코드 all pass에 도달하며 지식을 쌓기위한 주간이다. 만 줄이 넘어가는 코드를 분석하는 경험은 토이 프로젝트만 반복하던 주니어 개발자에겐 챌린지겠지만 일반적인 부트캠프가 줄 수 있는 지식과는 분명히 다른 차원의 지혜를 얻어갈 것이라고 기대한다. 

7주차는 추석연휴로 인해 1.5주로 연장되었다. 따라서 7주차는 10월 3일 화요일이 마감이다. 그 이후로 모든 주차는 주의 마감이 화요일이 된다는 점도 유의하자.

이번 주차의 팀은 박세준, 조가람, 최승현 이렇게 세 명이다. 오전 10시에 모여 화이팅하고 그날 저녁(시간미정)에 모여 브리핑을 할 것이다. 학습기간 동안에는 공부한 것들에 대한 내용을 다루고 구현기간 동안에는 다 함께 머리를 맞댄 결과물에 대해서 이야기를 나누게 될 것 같다.

추석기간에 빠지는 날이 팀원들이 서로 다르다. 
- 박세준: 2023-09-27(저녁) ~ 2023-09-30까지
- 조가람: 2023-09-30 ~ 2023-10-01 빠진다. 
- 최승현: 하루 날잡고 등산할 예정.

**권영진 교수님의 OS 강의**

- 2023-09-26T10:30:00
- 2023-10-10T10:30:00
- OS abstraction 개념에 초점을 맞추어 진행.
- [Pintos_1.pdf](https://drive.google.com/file/d/1rr1VobnaR8QiWq3TVImvzzHWWdB5d4B5/view)
- [01_os_review.pdf](https://drive.google.com/file/d/1v7ZT0uCqnSFQQY3jQsnXnCh9WHPpgQxZ/view)
- [CS 6200: Introduction to Operating Systems Course Videos (Georgia Tech College of Computing)](https://omscs.gatech.edu/cs-6200-introduction-operating-systems-course-videos)

## week07

- <https://youtu.be/myO2bs5LMak?si=jZeqP0oC_rdzYda9>
- [[prj1.threa ~ 2023-10-02ds.introduction {pintos}]]
- `pintos -- -q`: 실행 할 거 다 하면 자동으로 종료 (Ctrl+C 한다고 안꺼짐)

[[2023-09-23 dump {swjungle} {priority-scheduling} {alarm-clock}]]

[[2023-09-28 dump {swjungle}]]

**2023-09-29**

- [[priority-donate-multiple {swjungle} {pintos}]]
- [[priority-donate-nest, chain {swjungle} {pintos}]]

**2023-10-01 ~ 2023-10-02**

- [[Multi Level Feedback Queue {swjungle}{pintos}]]
- [[week07 WIL 정리, 발표준비 {swjungle}]]

**2023-10-03**

- [[priority-donate-sema {swjungle}{pintos}]]
- [[priority inversion on lock release {swjungle}{pintos}]]

## [[2023-09-26 권영진 교수님의 OS 강의 (1차) {swjungle}]]
