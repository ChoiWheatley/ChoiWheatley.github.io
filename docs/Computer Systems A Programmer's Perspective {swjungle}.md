---
aliases: 
tags: 
description:
title: "Computer Systems A Programmer's Perspective {swjungle}"
created: 2023-08-25T16:47:45
updated: 2023-08-27T19:10:44
---

## INDEX / README

- 본 페이지는 swjungle WEEK 01 ~ 03 기간도중 읽어야 할 챕터들 + 이후에 계속 접하면서 새로 알게된 내용 위주로 작성할 예저입니다. 페이지의 양이 많아지는 것을 줄이기 위해 어느정도 길이가 길어진다면 별도의 페이지를 파 그쪽으로 레퍼런싱을 하는 쪽으로 관리할 예정입니다.

## 1. A Tour of Computer Systems

### Overview

- 1.1 Information is bits + context
	- 정보는 비트 + 컨텍스트이다. 정보들이 0과 1 이진체계로 이루어져있음을 알려줄 것 같다. 0과 1을 전기적으로 표현하는 방법을 이야기하지는 않을 것이다. 이 책은 하드웨어를 다루는 책이 아니니까. 다만 어떻게 데이터와 명령어가 동일한 엔티티로 참조될 수 있는지 나올 것 같다.
- 1.2 Programs are translated by other programs into different forms
	- "프로그램은 다른 프로그램에 의해 서로 다른 형태로 변환된다."
	- 컴파일 과정에 대한 내용을 다루는갑다. 프로그램(C, JAVA 코드 등)이 다른 프로그램(링커, 컴파일러, 어셈블러 등)에 의해 바이너리로 변환되는 과정을 세부적으로 탐구하며 우리가 단순히 GCC를 써서 문자열을 컴퓨터가 알아먹을 수 있게 만들 수 있었는지에 대한 팩트폭격이 가해질 것 같다.
- 1.3 It pays to understand how compilation system work
	- 어떻게 컴파일 시스템이 작동하는지에 대한 내용을 다룰 것 같다. 1.2와의 차이점? 1.2는 스크립트 언어와 컴파일 언어와의 차이점에 대해서 이야기하려나?
- 1.4 Processors read and interpret instructions stored in memory
	- "프로세서는 메모리에 저장된 명령어를 읽고 해독한다."
- 1.5 Caches matter
	- 캐시가 진짜로 중요하기는 하지. 캐시의 양을 늘리는 것만으로 수없이 많은 성능향상을 볼 수 있기 때문!
- 1.6 Storage devices form a hierarchy
	- 스토리지 디바이스 (저장장치)는 계층 구조를 이룬다.
	- 스토리지라고 해서 단순 HDD, SSD만 스토리지라고 부르는게 아니라, 주기억장치라고 불리우는 메모리, 캐시들도 전부 스토리지 디바이스라고 부른다.
- 1.7 The operating system manages the hardware
	- 운영체제는 하드웨어를 관리한다.
	- 하드웨어 전반을 관리하고 적절한 자원을 프로세스에게 제공하고 권한을 부여하고 새로운 하드웨어가 인식 되었을 때 PnP를 지원하기 위해 어떤 방식으로 운영체제가 일을 하는지
- 1.8 Systems communicate with other systems using networks
	- 네트워크 하에 놓인 단일 컴퓨터는 라우터를 통하여 수많은 시스템을 마주할 수 있다. 웹 없이는 컴퓨터의 발전도 없었을 것이다. 네트워크 7계층에 대한 이야기가 나오려나?
- 1.9 Important themes
- 1.10 Summary

p.73 ~ p.131 | 60 pages

### 1.1 Information is bits + context

> The only thing that distinguishes different data objects is the context in which we view them.

서로 다른 데이터임을 구별할 수 있는 유일한 방법은 그것을 바라보는 문맥 뿐이다.

### 1.2 Programs are translated by other programs into different forms

- 전처리기
	- 헤더파일 포함, 각종 매크로 치환 후 텍스트 파일 출력
- 컴파일러
	- C 코드를 CPU 아키텍처와 맞는 명령어셋의 어셈블리 언어로 출력. (텍스트 파일)
- 어셈블러
	- 어셈블리 언어를 기계어로 변환하고 목적파일로 출력 (이진파일)
- 링커
	- 이진파일로부터 실행 가능한 파일을 만들기 위해 

### 1.3 It pays to understand how compilation system work

### 1.4 Processors read and interpret instructions stored in memory

### 1.5 Caches matter

### 1.6 Storage devices form a hierarchy

### 1.7 The operating system manages the hardware

### 1.8 Systems communicate with other systems using networks

### 1.9 Important themes

### 1.10 Summary

## 2. Representing and Manipulation Information
