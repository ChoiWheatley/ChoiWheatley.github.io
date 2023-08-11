---
aliases: 
tags: 
description:
title: week 01~03 {swjungle}
created: 2023-08-10T16:27:48
updated: 2023-08-11T19:20:27
---

# INDEX

- [swjungle 정보페이지](https://jungle7-7610626261f4.herokuapp.com/pages/W01-problem-solving.html)
- [문제모음집](https://docs.google.com/spreadsheets/d/1z4a3pSM-h76kwdUAlPXXmbwcpP-VKFYFK92TCQtjXV4/edit#gid=0)
	- [문제모음집{개인}](https://docs.google.com/spreadsheets/d/1G0WlH6y9V1RL09KuCbqwM-E57b5xTaIL9HiaQ2ZIvJs/edit?usp=sharing)

# 컴퓨팅사고로의 전환

개발자는 컴퓨터에게 일을 잘 시켜야 한다. 그럼 일을 잘 시킨다는 건 뭐임? 

- Halting Problem: 컴퓨터가 답을 내게 만들어야 함 (멈춰야 함.) 무한루프는 생각보다 치명적인 에러임

이미 컴퓨터가 풀 수 있는 문제들을 여러분에게 주어지고 그 문제를 풀 능력을 기르는 것이 이번 주차의 목표.

백준문제 풀기: 키워드 위주로. 기초문제는 오늘 안에로 다 풀어주시고. 파이썬 언어로 풀기를 바람

==시험==

- 매주 목요일 오전 10:00 ~ 11:30
- 깃헙에 풀 리퀘스트를 생성하여 제출.
- 풀이를 리뷰할 사람을 정해 코드리뷰.

==CSAPP==

- WEEK01: ~1.4. 프로세서는 메모리에 저장된 인스트럭션을 읽고 해석한다

OS 단에 들어가기 전에 미리미리 하기 위한 저시기임.

# WEEK01 특별한 과제

블로그를 하나 만들어 에세이를 작성하시오. ~= 12일 자정까지

# TIPS

- python 관련
	- [[20230501 estsoft - python - convention, types, variables, int, float|floating point formatting]]
	- [[iterator를 꺼내 문자열로 만드는 방법{sof}]]
	- [[lower bound with bisect_left (python)]]
	- [[20230517 estsoft - python - linked list - dataclass - typing Self cast type union - __getitem__ - slice.indices|colon slicing]] 2588
	- 
str \* 연산자  
`a, b = [1, 2, 3, 4]` => 오류남

# Chapter01 알고리즘 기초

- 순차구조(sequential)와 선택(selection)구조
	- 순차: 한 문장씩 처리
	- 선택: 조건에 따라 실행 흐름 변경
- 형변환 to integer, floating point
	- `int(문자열, 진수)`, `float(문자열)`
- 헤더와 스위트(suite)
	- if, function, loop 등 복합문은 `:`를 기준으로 헤더와 스위트로 나뉘어진다.
	- 스위트는 평시에는 헤더의 다음줄의 공백 4개를 들여쓰기로 사용하지만, 단순문일 경우 같은 줄에 늘여써도 된다. 이때, 단순문을 두 개 이상 사용하는 것도 가능.
	- 단순문의 정의: 복합문이 아닌 모든 구문
- What is the algorithm?
	- 문제를 해결하기 위해 기술된 일련의 실행절차
- Decision Tree (결정트리)
	- 세 정수 a, b, c의 대소관계조합을 모두 나열해보면 트리형태를 띄게 된다. 이것을 결정트리라고 부름.
