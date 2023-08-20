---
aliases: 
tags: 
description:
title: week 01~03 {swjungle}
created: 2023-08-10T16:27:48
updated: 2023-08-20T16:30:11
---

## INDEX

- [swjungle 정보페이지](https://jungle7-7610626261f4.herokuapp.com/pages/W01-problem-solving.html)
- [문제모음집](https://docs.google.com/spreadsheets/d/1z4a3pSM-h76kwdUAlPXXmbwcpP-VKFYFK92TCQtjXV4/edit#gid=0)
	- [문제모음집{개인}](https://docs.google.com/spreadsheets/d/1G0WlH6y9V1RL09KuCbqwM-E57b5xTaIL9HiaQ2ZIvJs/edit?usp=sharing)
- [swjungle-week-01 {GH}](https://github.com/ChoiWheatley/swjungle-week-01)
- [P7-Week01-03 목요일 시험 문제 답안 제출용 리포지토리 {GH}](https://github.com/SWJungle/P7-Week01-03)
- [[Doit 자료구조와 함께 배우는 알고리즘 기초 파이썬 편]]

## 컴퓨팅사고로의 전환

개발자는 컴퓨터에게 일을 잘 시켜야 한다. 그럼 일을 잘 시킨다는 건 뭐임? 

- Halting Problem: 컴퓨터가 답을 내게 만들어야 함 (멈춰야 함.) 무한루프는 생각보다 치명적인 에러임 이미 컴퓨터가 풀 수 있는 문제들을 여러분에게 주어지고 그 문제를 풀 능력을 기르는 것이 이번 주차의 목표.

백준문제 풀기: 키워드 위주로. 기초문제는 오늘 안에로 다 풀어주시고. 파이썬 언어로 풀기를 바람

==시험==

- 매주 목요일 오전 10:00 ~ 11:30
- 깃헙에 풀 리퀘스트를 생성하여 제출.
- 풀이를 리뷰할 사람을 정해 코드리뷰.

## WEEK01

- [x] [[2023-08-12 SWJUNGLE 입성 후 쓰는 첫 데일리 브리핑|블로그를 하나 만들어 에세이를 작성하시오. ~= 12일 자정까지]]

### TIPS

[[0014 Python 🐍]]으로 옮겨적을것
___
- [[20230501 estsoft - python - convention, types, variables, int, float|floating point formatting]]
- [[iterator를 꺼내 문자열로 만드는 방법{sof}]]
- [[lower bound with bisect_left (python)]]
- [[20230517 estsoft - python - linked list - dataclass - typing Self cast type union - __getitem__ - slice.indices|colon slicing]] boj 2588
- `my_single_element_tuple = 1,`
- `this_is_not_a_tuple = (1)`
- [?] `a, b = [1, 2, 3, 4]` => 오류남 참조할 곳이 없게 되어서. None과 null의 차이가 뭐냐?
	- 리턴문이 없는 함수는 None을 리턴하는데, 왜 쟤네들은 에러냐?

### 문제풀이

[[0011 Algorithms ♾️#문제풀이 회고용]] 페이지에 추가할 것

- [swjungle-week-01 {GH}](https://github.com/ChoiWheatley/swjungle-week-01)를 참고하세요. 문제푼것들, 교재 실습한 것들 코드 위주로 올라갑니다. 

- [ ] [[next_permutation 구현#일곱 난쟁이]] 직접 next_permutation 구현해 볼 것
- [x] [[10819 차이를 최대로 {boj} {permutation}]]
- [x] [[10927 외판원 순회 2 {boj} {완전탐색}]]
- [x] [[1629 곱셈 {boj} {modular exponentiation} {나머지 지수승}]]
- [ ] [[10830 행렬제곱 {boj}]] 행렬 대각화 사용하여 풀어볼 것
- [x] [[2805 나무자르기 {boj}]]
- [ ] [[2110 공유기 설치 {boj} {parametric search}]]

## WEEK02

- 스택, 큐, 우선순위큐, 그래프(vertex, edge, node, arc), BFS, DFS, 위상정렬
- 읽어야 할 단원
	- 4-1 스택
	- 4-2 큐
	- 6-8 힙 정렬
	- 9-1 트리
	- 9-2 이진트리와 이진검색트리
- CSAPP
	- 1.5 캐시가 중요하다
	- 1.6 ?
	- 1.7 운영체제는 하드웨어를 관리한다
- Day01: Stack
	- [PR: 10828, 17608](https://github.com/ChoiWheatley/swjungle-week-02/pull/2)
- Day02: Queue
	- [PR: 17608, 10828, 2504, 2493, 2812, 18258, 2164 ](https://github.com/ChoiWheatley/swjungle-week-02/pull/8)
- Day03: PriorityQueue, Graph
	- [PR1: 원 영역, 최대 힙](https://github.com/ChoiWheatley/swjungle-week-02/pull/11)
	- [PR2: 가운데를 말해요, 카드 정렬하기](https://github.com/ChoiWheatley/swjungle-week-02/pull/13)
- Day04: DFS
	- [PR](https://github.com/ChoiWheatley/swjungle-week-02/pull/15)

### Doit!

[[Doit 자료구조와 함께 배우는 알고리즘 기초 파이썬 편#Chapter04 스택과 힙]]

### TIPS

- `Exception`을 상속하는 클래스는 `raise`가 가능해지는구나 ==p.157==
- `try` - `except` - `else` - `finally` 
	- `try`: 원래 처리할 내용
	- `except`: 예상 에러 발생시 처리할 내용
	- `else`: 에러 미발생시 처리할 내용
	- `finally`: 심지어 중간에 리턴을 했을지라도 에러 발생여부 상관없이 무조건 실행하는 내용
- `BaseException`과 `Exception` 간의 차이
- `__contains__`: `in` 문법 사용 가능
- [[dataclass ValueError mutable default class list is not allowed, use default_factory {python}]]

### 문제풀이

- [[2812 크게 만들기 {boj} {stack}]] -> int, str 형변환을 자주 하지 말자
- [[18258 큐 2 {boj} {queue}]]
- [[11279 최대 힙 {boj} {heap} {python class Heap}]]
- [[1655 가운데를 말해요 {boj} {heapq}]]
- [[1715 카드 정렬하기 {boj}]] →수열을 더하는데 괄호를 어디에 배치하느냐에 따라 달라지는 결과값 중 최소를 구하는 문제인듯

### 다시 풀 문제

- [[13334 철로 {boj}]] → 길이 d의 모든 선분 L에 대하여 집과 사무실의 위치가 모두 L에 포함되는 사람들의 최대 수를 구하는 문제가 힙이야?
- 11725(트리부모찾기): 멍청하게 10_000 ** 2 크기의 행렬을 생성하려고 했다.
- 1707(이분그래프): 20분동안 문제를 다르게 바라보았다. 그래도 실마리는 찾은 것 같다.  
	집합 안에서 정점끼리 인접하지 않아야 한다. DFS 돌면서 핑퐁하면서 집합을 다르게 준다. 두 개의 집합으로부터 중복되는 원소가 있다면 `set.교집합()` 결과가 비어있지 않으면 이건 이분그래프가 아니게 된다는 것임
- 21606(아침산책): 73점, 
	- Ai = 0인 케이스, Ai = 1인 i는 2개 이하인 케이스, N<= 100인 케이스는 모두 맞추었으나
	- 1 <= i < N 인 모든 정수 i에 대해 트리의 i번 정점과 i+1번 정점 사이에 간선이 존재하는 경우 (35점)과 추가 제한조건 없는(92점) 케이스는 틀렸다.
