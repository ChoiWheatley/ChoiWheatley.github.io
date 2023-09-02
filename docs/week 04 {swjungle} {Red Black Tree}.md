---
aliases: 
tags: 
description:
title: week 04 {swjungle} {Red Black Tree}
created: 2023-08-31T13:54:10
updated: 2023-09-02T10:20:37
---

## INDEX

- [[0120 swjungle 🤖]]
- [4주차 herokuapp.com](https://jungle7-7610626261f4.herokuapp.com/pages/W04-rbtree.html)
- [rbtree-lab {GH}](https://github.com/SWJungle/rbtree-lab)
- [missing semester-kr](https://missing-semester-kr.github.io/) | CS 학기에서 가르쳐주지 않지만 거의 필수적으로 알아야 하는 주제들에 대한 내용을 다루고 있음.

## 과제의 의도 (Motivation)

- 복잡한 자료구조(data structure)를 구현해 봄으로써 자신감 상승
- C 언어, 특히 포인터(pointer)와 malloc, free 등의 system call에 익숙해짐.
- 동적 메모리 할당(dynamic memory allocation)을 직접 사용해 봄으로써 동적 메모리 할당의 필요성 체감 및 data segment에 대한 이해도 상승
- 고급 언어에서 기본으로 제공되는 자료구조가 세부적으로는 어떻게 구현되어 있는지 경험함으로써 고급 언어 사용시에도 효율성 고려

## 참고문헌

- [위키백과: 레드-블랙 트리](https://ko.wikipedia.org/wiki/%EB%A0%88%EB%93%9C-%EB%B8%94%EB%9E%99_%ED%8A%B8%EB%A6%AC) ([영어](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree))
- CLRS book (Introduction to Algorithms) 13장 레드 블랙 트리 - Sentinel node를 사용한 구현
- [Wikipedia:균형 이진 트리의 구현 방법들](https://en.wikipedia.org/wiki/Self-balancing_binary_search_tree#Implementations)

## Before start...

일주일을 어떻게 보낼지에 대한 메타적인 윤곽을 보자. 스터디를 하되, 실제 구현은 개인의 몫이라고 한다. 탐험준비 챕터에 대략적인 아웃라인이 주어진다.

==당장 해야할 것==

- [x] WSL Ubuntu 20.04 -> 22.04로 업그레이드 하기
- [x] 개발환경 설치
- [x] 과제 다운 및 복제

==탐험준비==

- C언어 공부, 
	- 문법 공부 후 linked list를 이용한 ㅣist, queue, tree 구현 등 다양한 자료구조를 직접 구현하고 **라이브러리를 만들어 가면서** 
- Linux/GCC 사용법 익히기, 
- RB tree 이론, 
- malloc/free 사용법 파악, RB tree 구현 순서로 진행
- rbtree-lab 리포의 메이크파일에 있는 `make test` 를 실행하여 통과하기


~~설명할 것들을 나눠서 공부하고 돌아와서 저시기를 한다~~. 다 같이 같은 범위를 공부하고 다 같이 발표한다.

시간 맞추기 (오전 10시) 일요일 제외 -> 늦잠을 안 자게 된다.

**금요일까지**

- C에 대한 기본문법 학습 | [[0017 C 🍎]]
	- static, ~~array, string, multi-dimension array, string array, pointer,~~ structure, dynamic memory allocation, conditional compile, preprocessors

**토요일**

- [ ] malloc, calloc, realloc, struct
- [ ] rbtree python code 
	- CLRS book 13장 레드블랙 트리의 센티널 노드를 사용한 구현 참조할 것
- [ ] [[2의 보수법]] 복습

> [!note] RB-tree, malloc-lab을 동시에 공부하고 Python으로 구현을 시도해보는 것으로 무엇이 필요한지에 대한 인사이트를 얻을 수 있을 것이다.
