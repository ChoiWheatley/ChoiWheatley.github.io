---
aliases: 
tags: 
description:
title: kaist pintos assignment specification {casys-kaist.github.io}
created: 2023-09-22T10:42:50
updated: 2023-09-22T19:26:12
---
- <https://casys-kaist.github.io/pintos-kaist/>
___

## Overview

1. **소개 (INTRODUCTION):** 이 섹션에서는 전반적인 내용 소개와 시작 방법, 평가 방법, 법적 및 윤리적 문제에 대한 정보를 제공합니다.
    
2. **프로젝트 1: 스레드 (PROJECT1: THREADS):** 스레드와 관련된 프로젝트에 대한 정보를 제공합니다. 각 프로젝트는 다음과 같은 하위 섹션으로 구성됩니다:
    
    - **소개 (Introduction):** 해당 프로젝트의 개요를 설명합니다.
    - **알람 시계 (Alarm Clock):** 알람 시계 구현에 대한 정보를 제공합니다.
    - **우선 순위 스케줄링 (Priority Scheduling):** 우선 순위 스케줄링과 관련된 내용을 다룹니다.
    - **고급 스케줄러 (Advanced Scheduler):** 고급 스케줄링 기법에 대한 정보를 제공합니다.
    - **자주 묻는 질문 (FAQ):** 해당 프로젝트와 관련된 자주 묻는 질문에 대한 답변을 제공합니다.
	
3. **프로젝트 2: 사용자 프로그램 (PROJECT2: USER PROGRAMS):** 사용자 프로그램과 관련된 프로젝트에 대한 정보를 제공합니다. 이 역시 각 프로젝트는 소개, 인수 전달, 사용자 메모리, 시스템 호출, 프로세스 종료 메시지, 실행 파일에 대한 쓰기 거부 등과 관련된 세부 정보를 제공하며 FAQ 섹션도 포함되어 있습니다.
    
4. **프로젝트 3: 가상 메모리 (PROJECT3: VIRTUAL MEMORY):** 가상 메모리와 관련된 프로젝트에 대한 정보를 제공합니다. 메모리 관리, 익명 페이지, 스택 확장, 메모리 매핑 파일, 스왑 인/아웃, 복사를 통한 쓰기 등에 대한 내용을 다루며 FAQ 섹션도 포함되어 있습니다.
    
5. **프로젝트 4: 파일 시스템 (PROJECT4: FILE SYSTEM):** 파일 시스템과 관련된 프로젝트에 대한 정보를 제공합니다. 색인화된 파일 및 확장 가능한 파일, 하위 디렉토리와 소프트 링크, 버퍼 캐시, 동기화 등에 대한 내용을 다루며 FAQ 섹션도 포함되어 있습니다.
    
6. **부록 (APPENDIX):** 스레드, 동기화, 메모리 할당, 가상 주소, 페이지 테이블, 디버깅 도구, 개발 도구, 해시 테이블 등과 같은 부록 내용을 포함합니다.

## INTRODUCTION

## PROJECT1: THREADS

- prerequisites
	- [[synchronization {pintos}]]에 있는 동기화 관련 코드 스니펫 읽으면서 흐름 이해
- objectives
	- [Alarm Clock](./doc/alarm_clock.md)
	- [Priority Scheduling](#)
	- [Advanced Scheduler(option)](#)
