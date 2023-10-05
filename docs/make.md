---
aliases: 
tags: 
description:
title: make
created: 2023-09-15T13:44:24
updated: 2023-10-05T09:51:52
---
- [[0110 Utility 🔧]]
___

## [Make 기반 빌드 시스템 (1): 빌드 시스템?](http://developinghappiness.com/?p=26)

make가 할 수 있는 일은...

1. 여러 타겟 플랫폼들을 하나의 명령줄로 생성해낼 수 있음.
2. 다양한 빌드옵션들을 사전에 정의하여 일관적이고 동일하게 옵션을 적용할 수 있음.
3. 최종 실행 라이브러리/실행파일을 빌드라는 개념으로 추상화할 수 있음.
4. 자동화된 단위 테스트를 만들 수 있음.

## [Make 기반 빌드 시스템 (2): Makefile 기초]

## [Make 기반 빌드 시스템 (3): 소스 목록 관리를 조금 편리하게](http://developinghappiness.com/?p=174)

## 자동변수

<http://korea.gnu.org/manual/4check/make-3.77/ko/make_10.html#SEC97>

## make faster with `-j $(nproc --all)` option

`-j`에 대한 설명은 [gnu.org :: Parallel](https://www.gnu.org/software/make/manual/html_node/Parallel.html)에서 확인바람. `nproc`에 대한 설명은 [다음 stack overflow 답변](https://stackoverflow.com/a/17089001/21369350)을 참고. 현재 시스템의 CPU 수만큼의 job을 생성해 병렬적으로 컴파일하게된다.
