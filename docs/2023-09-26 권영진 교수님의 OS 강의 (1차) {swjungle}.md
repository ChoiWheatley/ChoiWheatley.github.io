---
aliases: 
tags: 
description: 질문 개많으니까 도움 많이 된다
title: 2023-09-26 권영진 교수님의 OS 강의 (1차) {swjungle}
created: 2023-09-25T20:32:05
updated: 2023-09-26T10:53:37
---
- [[week07-10 {swjungle} {pintos}]]
- [Pintos_1.pdf](https://drive.google.com/file/d/1rr1VobnaR8QiWq3TVImvzzHWWdB5d4B5/view)
- [01_os_review.pdf](https://drive.google.com/file/d/1v7ZT0uCqnSFQQY3jQsnXnCh9WHPpgQxZ/view)
- [CS 6200: Introduction to Operating Systems Course Videos (Georgia Tech College of Computing)](https://omscs.gatech.edu/cs-6200-introduction-operating-systems-course-videos)
___

## README

양질의 질문이 진짜로 겁나 많다. 강의 전에 직접 답변해보고, 강의 들으면서 꼬리질문과 그 답변을 붙여나가자.

## 01_os_review.pdf preview

[[01_os_review.pdf {swjungle}]]

## 2023-09-26 OS 강의

카이스트는 올해 처음으로 핀토스 폐지...? 부임해서 32비트 버전을 64비트 버전으로 업그레이드를 했다. 바꾼 이유는 바로 cheating 때문이었다. 학생들이 핀토스를 얼마나 잘 이해했느냐 이걸 4년 했더니 또 cheating + ChatGPT로 랩 이라는게 의미가 없어졌다. GPT한테 물어보세요 이젠. 핀토스가 없는데 이젠 과제를 어떻게 내냐? 캐시 프리패싱을 만들어 보세요, 성능을 향상시켜보세요 등등. 어떤 카이스트 교수님은 그래서 숙제를 3배로 늘려버렸다.

GPT와 함께 공부하세요. 모든 상상력을 발휘해 GPT에게 질문할 것.

- **OS 지식**. 가상 메모리가 뭔지, 스레드가 뭔지 공부를 한다. 
- **디자인 지식** OS 기본적인 특징과 왜 그렇게 만들었는지에 대한 추측. 엄청 추상적이고 뜬구름 잡는 얘기처럼 들린다. WHY 이렇게 만들어졌는가? 에 더 중요하게 본다. GPT는 여기에 대답을 제대로 못한다. 

Bottom Up으로 정의하는 방식이 우리가 일반적으로 하는 방법. 하지만 OS는 Top Down이다. OS에 이런 속성을 넣고 싶어, OS에 뭔가 필요한 기능이 나왔어. 멀티프로그래밍에 대한 요구가 생겨나 스레드라는 개념이 생겨남. 스레드는 그럼 lightweight해야겠지?

오늘 시간에는 디자인 지식을 기반으로 대화할 예정이다.

OS는 하드웨어의 복잡성을 감추기 위해 탄생. 그걸 몰라? 그럼 일단 인터페이스만 쓰겠지. 이 감추는 방법들이 어떻게 동작하는지 이해한다면 컴퓨터가 어떻게 동작하는지 대충 이해할 수 있다. 감춤에 대한 이야기

mmap은 요청한 공간을 바로 할당해주지 않는다.
