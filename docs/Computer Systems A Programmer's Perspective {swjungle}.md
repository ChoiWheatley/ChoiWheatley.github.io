---
aliases: 
tags: 
description:
title: "Computer Systems A Programmer's Perspective {swjungle}"
created: 2023-08-25T16:47:45
updated: 2023-08-27T23:28:32
---

## INDEX / README

- 본 페이지는 swjungle WEEK 01 ~ 03 기간도중 읽어야 할 챕터들 + 이후에 계속 접하면서 새로 알게된 내용 위주로 작성할 예저입니다. 페이지의 양이 많아지는 것을 줄이기 위해 어느정도 길이가 길어진다면 별도의 페이지를 파 그쪽으로 레퍼런싱을 하는 쪽으로 관리할 예정입니다.
- [[Operating Systems {ssu 2021-second-semester archive}]]

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
	- 이진파일로부터 실행 가능한 파일을 만들기 위해 relocatable object들의 주소를 정확한 위치로 할당시킨다.
		- **relocatable object란?**: 정적 라이브러리나 동적 라이브러리들이 정의한 변수, 함수등을 가리키는 포인터. 테이블의 형태로 존재하고 링킹작업 전에는 말 그대로 비어있다.

**추가**

- 로더
	- 실행파일을 메모리에 올리고 PC(Program Counter)를 설정하는 프로그램. COW (Copy On Write) 메모리 주소 참조, 동적 라이브러리 로드도 담당한다.

### 1.3 It pays to understand how compilation system work

컴파일이 진행되는 방법을 알면 코드를 최적화 시킬 수 있고, 보안취약점으로부터 안전한 코드를 작성할 수 있으며, 링크 타임 에러를 해결할 수 있게 된다.

### 1.4 Processors read and interpret instructions stored in memory

셸의 역할: 사용자의 명령을 입력으로 받아 다양한 일을 수행하는 프로그램. 실행 가능한 프로그램을 실행시켜 새 프로세스로 만들어주는 역할을 담당한다. 

**시스템을 구성하는 하드웨어 컴포넌트**
- Buses
- I/O Devices
- Main Memory
- Processor

> Thus, we can distinguish the processor's instruction set architecture, describing the effect of each machine-code instruction, FROM its microarchitecture, describing how the processor is actually implemented.  
> 따라서, 우리는 프로세서의 설계도인 마이크로아키텍쳐와 각각의 기계어가 의미하는 바를 설명하는 명령어 셋 아키텍처 간의 차이를 구별할 수 있습니다.

**DMA**  
Directed Memory Access라고, 우리가 영속저장장치에 저장된 실행 가능한 파일을 실행시킬 때 CPU를 거치지 않고 바로 메모리에 직빵으로 올려버리는 테크닉이 있다.

### 1.5 Caches matter

저장장치는 크기가 클수록 느리고, 멀수록 느리다. CPU가 데이터를 처리하는 속도의 증가량보다 메모리가 데이터를 처리하는 속도의 증가량이 느리기 때문에 이 차이는 시대가 지날수록 커진다. 따라서 메인 메모리와 CPU 사이에 캐시를 다단계로 갖추어 중요하고 자주 접근할 데이터들을 빠르게 접근할 수 있도록 만들었다.

**localities**
- 공간적 지역성
	- 한 메모리를 참조하게 되면 그 주변도 같이 참조할 가능성이 높다.
- 시간적 지역성
	- 한 메모리를 참조하게 되면 다시 참조될 가능성이 높다.

### 1.6 Storage devices form a hierarchy

register > L1 cache > L2 cache > L3 cache > main memory > disk storage > network

### 1.7 The operating system manages the hardware

프로그램이 실행될 때 우리는 하드웨어를 독점적으로 사용하고 있다는 착각을 하게 만든다. 그 기저엔 운영체제가 깔려있고, 운영체제는 레이어로 구현되어있다. 하드웨어를 다루는 작업 (`printf`, `scanf` 등등)을 수행할 때에도 하드웨어를 컨트롤 하는 레이어의 인터페이스를 사용하는 수밖에 없다. 레이어 간의 추상화 덕분에 보안상의 이점도 가져가고, 인지부하도 줄일 수 있게 되었다.

**다양한 추상화들**
- ISA(Instruction Set Architecture)는 기계의 동작을 추상화했다.
- 프로세스는 프로세서를 추상화했다. context switching 덕분에, 프로세서의 개수에 제약을 받지 않고 원하는 만큼의 프로세스를 실행할 수 있게 되었다.
- 파일은 디스크 IO 디바이스를 추상화했다.
- 가상메모리는 디스크와 메인 메모리를 추상화했다.
- 가상 운영체제는 위의 모든 것을 추상화했다. (개쩌는데?) 😮

**프로세스**  
`syscall`은 커널 코드와 유저 코드를 분리하는 인터페이스이다

**스레드**  
하나의 프로세스 안에서도 여러개의 실행흐름이 존재할 수 있다.

**가상 메모리**
- [?] 왜 스택이 위에서 아래로 자라고 힙이 아래에서 위로 자라지? `brk`는 어디에 붙어있더라?  
![[스크린샷 2023-08-27 오후 9.33.41.png]]

- Program Code, Data (text 영역)
- Heap 영역
- Shared Libraries영역이 가상메모리의 중간에 존재한다고? 들어본 적이 없는데?
- Stack 영역은 모든 함수와 지역변수들이 쌓이는 공간이다.
- 커널 가상 메모리 영역은 커널코드를 직접 손대는 것으로부터 방지하기 위해 마련된 공간이다.

**Files**

> 유닉스 시스템에서 모든 것은 파일이다. IO기기나 네트워크도 파일이라는거. 

### 1.8 Systems communicate with other systems using networks

WWW, FTP, SSH 등 다양한 애플리케이션들이 네트워크 어댑터를 사용하여 통신한다.

### 1.9 Important themes

- Amdahl's Law(암달의 저주)
	- 병목현상으로 인해 한 컴포넌트의 성능향상이 실제로 유의미한 변화를 주기 어렵다. $T_{old}$ 시간이 걸리던 한 시스템 중 일부를 개선하여 $k$ 배 성능향상을 이뤄냈다고 해보자. 만약 이 컴포넌트 실행시간이 전체 시스템의 $\alpha$ 만큼의 비중을 가지고 있다면 전체 시스템 성능향상 $T_{new}$은 어느정도일까?

	$$
	\begin{align}
		T_{new} &= (1-\alpha)\cdot T_{old} ~+~ \frac{\alpha \cdot T_{old}}{k} \\
		&= T_{old} ((1-\alpha)+\frac{\alpha}{k})
	\end{align}
	$$

	- 그래서 전체 시스템의 60퍼센트의 실행시간을 차지하는 한 컴포넌트를 3배 빠르게 만들었을 때 전체 시스템의 속도는 이전에 비해 1/(0.4 + 0.6 / 3) = 1.6667 배 빨라지게 된다. 

- Concurrency and Parallelism
	- Thread Level Parallelism: 멀티 프로세서로 인해 컴퓨터는 동시에 수행할 수 있는 작업의 양이 늘어났다. 하이퍼스레딩 기술을 사용하여 코어 하나당 두개의 스레드를 동시에 실행하는 것을 보장한다. 
	- Instruction Level Parallelism: Superscala Processors라고 불리우는 파이프라이닝을 통해서 클록 사이클 수회를 차지하는 명령어를 계단 형태로 배치하여 한 사이클 당 하나의 명령어가 실행 완료가 되게 구성할 수 있다.
	- [!] Single Instruction Multiple data (SIMD): 한 번에 8개의 실수연산을 수행하도록 하드웨어가 구성되어있어 보다 빠르게 행렬연산을 수행할 수 있다. GCC 컴파일러는 C 코드로부터 SIMD 명령어를 식별해 낼 수 있다.

- Layer and Abstractions

    > 소프트웨어 공학의 역사는 추상화 수준을 높히는 방향으로 진행되어왔다. |  [[20230522 김충환 회고강사특강#협력을 통한 추상화]] 

	- API (Application Programming Interface): 프로그래머들이 기능을 제공하기 위해 묶어놓은 함수나 클래스의 집합

### 1.10 Summary

## 2. Representing and Manipulation Information
