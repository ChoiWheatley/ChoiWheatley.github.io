---
aliases: 
tags: 
description:
title: week 01~03 {swjungle}
created: 2023-08-10T16:27:48
updated: 2023-08-11T20:20:58
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

[[0014 Python 🐍]]으로 옮겨적을것

- python 관련
	- [[20230501 estsoft - python - convention, types, variables, int, float|floating point formatting]]
	- [[iterator를 꺼내 문자열로 만드는 방법{sof}]]
	- [[lower bound with bisect_left (python)]]
	- [[20230517 estsoft - python - linked list - dataclass - typing Self cast type union - __getitem__ - slice.indices|colon slicing]] 2588
	- 
`a, b = [1, 2, 3, 4]` => 오류남

# Doit! 자료구조와 함께 배우는 알고리즘 기초 : 파이썬 편

## Chapter01 알고리즘 기초

- 순차구조(sequential)와 선택(selection)구조, 반복(iteration)구조
	- 순차: 한 문장씩 처리
	- 선택: 조건에 따라 실행 흐름 변경
	- 반복: 조건이 성립하는 동안 주어진 구간의 코드를 반복
		- 카운터용 변수 (`cnt`, `counter`, ...): 반복문을 제어할 때 사용되는 변수
- 형변환 to integer, floating point
	- `int(문자열, 진수)`, `float(문자열)`
- [[헤더와 스위트(suite) {python}]]
- What is the algorithm?
	- 문제를 해결하기 위해 기술된 일련의 실행절차
- Decision Tree (결정트리)
	- 세 정수 a, b, c의 대소관계조합을 모두 나열해보면 트리형태를 띄게 된다. 이것을 결정트리라고 부름.
- DRY (Don't Repeat Yourself) 
	- DRY 메커니즘의 예시: 하드코딩을 피하라 [[reverse를 사용해야 하는 이유 {django}]]
	- 논리적으로 일치하는 분기문을 갖다 쓰지 말라. `if a < b: ... \n elif b >= a: ...` 이딴거 쓰지 말라는 거임 ==p.25==
	- 9999번 A 분기에 들어가고 1번 B 분기에 들어갈거면 if 쓰지 말라. ==p.38==
	- 
- `if`구문에서 `else`를 쓰지 않으면 내부적으로 `else: pass`가 숨어있다.
- 연산자와 피연산자, operator, operand. 용어파악
	- 삼항 연산자 (ternary operator) : `x if x < y else y`
- swap: `a, b = b, a` 개쩔지 아니한가 : 사실 튜플 공간을 만들어 쓰는거라서... 변수를 하나 생성하여 스왑하는 것보다 비효율적임
- str `*` 연산자는 문자열을 복제하여 리턴한다.
