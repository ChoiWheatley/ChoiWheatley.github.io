---
links: "[[docs/index|index]]"
status: 
description: 
title: 0011 Algorithms ♾️
created: 2023-02-09T11:01:38
categories:
  - index
aliases:
  - algorithms
  - 알고리즘
tags:
  - index
  - algo
date created: Thursday, February 9th 2023, 11:01:38 am
date modified: Monday, February 27th 2023, 6:20:45 pm
updated: 2024-12-23T18:01:31
---

___

# ReadMe

알고리즘 문제는 어떻게 정리해야할지 아직 제대로 잡혀있지 않았다. 열심히 문제풀고 C++로 푼 문제는 [choi-workspace](https://github.com/ChoiWheatley/choi-workspace)에, 파이썬으로 푼 문제는 [ChoiSeungHyeon](https://github.com/OrmiCodeRanger/ChoiSeunghyeon) 에 정리하고 있는데, 이것을 인덱싱을 어떻게 잘 할 수는 없을까?

내가 가지고 있는 건 노션 사이트에서 열심히 정리해놓은 [이 페이지](https://choiwheatley.notion.site/algo-26c08b5f469647c4b138435c32501b4f)가 그나마 잘 보존되어있다. 하지만 어느순간부터 다시 손을 놓게 되었는데, 일단 문제풀기가 급급한 나머지 정리는 기약할 수 없는 먼 미래로 가져다 놓았기 때문이다. 쉬운 문제는 그런대로 넘어가도 좋지만 어려운 문제 또는 치트시트에 가져다 놓을 문제는 별도로 정리할 시간을 갖는 것이 좋을 것 같다.

그래서, 옵시디언은 모든 하이퍼링크의 시작이오, 풀이는 깃허브로, 설명은 노션(혹은 미래의 내 블로그로)으로 이동하도록 만드는 것이 현재 나의 목표이다.

**2023-08-13T15:25:38 수정**

노션이 너ㅓㅓㅓㅓㅓㅓㅓㅓㅓ무 느리다. 최근에 블로그 자동배포기능을 올려놓았으니, 아예 이 참에 옵시디언을 주력 노트메이킹 도구로 사용하자.  그러면 기존에 노션에 작성했던 것들은 여기로 옮기느냐? 그냥 노션페이지 링크를 걸어두는 걸로 만족하자.


---

# INDEX

---

- [[graph 기초|graph]]
	- [[Heap]]
	- [[BFS]]
	- [[Lowest Common Ancester, LCA|LCA]]
	- [[tree 기초|tree]]
	- [[Segment Tree]]
		- [[펜윅 트리(바이너리 인덱스 트리) Fenwick Tree]]
- String
	- [[kmp|kmp]]
	- [[LCS 가장 긴 공통 부분수열 {longest common subsequence}|LCS]]
	- [[트라이]]
- DP
	- [[LIS 가장 긴 증가하는 부분수열|LIS]]
	- [[LCS 가장 긴 공통 부분수열 {longest common subsequence}|LCS]]
- sort
	- [[merge sort]]
	- [[퀵정렬 {quick sort}|Quick Sort]]
	- [[도수정렬 (Counting Sort) {rust} {python}]]
- [[TSP - 외판원 순회 291cd070bc53494495b4456819043fa0|TSP]]
- [[next_permutation 구현|next permutation 구현]]
- [[종만북 카라츠바 알고리즘 정답편 + 포스팅ᄋ 5bb97600d3b94c38b1050e1cb4ee3c4e|카라츠바 알고리즘 정답편]]
- [[Hash]] 
- [[divide and conquer]]
- [[이분탐색]]
	- [[binary search를 활용한 lower upper bound 그리고 parametric search까지 {Notion export}]]
- [[단절점]]
- [[two pointer]]

---

# 문제풀이 회고용

```dataview
TABLE tags, difficulty, status, links FROM "docs/algorithms"
```

## Links

- problems
	- [최강박조 백준](https://www.acmicpc.net/group/5673)
	- [코드레인저 - ESTsoft 백준](https://www.acmicpc.net/group/17719)
	- [프로그래머스](https://school.programmers.co.kr/learn/challenges?order=recent)
	- [알고스팟](https://algospot.com)
	- [SWEA](https://swexpertacademy.com/main/main.do)
	- [leet code](https://leetcode.com)
		- [LeetCode 75](https://leetcode.com/studyplan/leetcode-75) - 기초 문제집
	- [백준](https://boj.kr)
- github
	- [ChoiSeungHyeon](https://github.com/OrmiCodeRanger/ChoiSeunghyeon) | Python 문제풀이 오르미 코드레인저 
	- [choi-workspace](https://github.com/ChoiWheatley/choi-workspace) | C++ 문제풀이 (파일구조가 조금 개판이긴 함)
		- [cpp-algorithms](https://github.com/ChoiWheatley/cpp-algorithms) | choi-workspace에서 c++ 알고리즘만 가져옴
	- [AlgorithmCheatSheet](https://github.com/OrmiCodeRanger/AlgorithmCheatSheet) | 오르미 코드레인저 팀원들이 사용하는 파이썬 기반 알고리즘 치트시트
	- [Algorithm-Study](https://github.com/ChoiWheatley/Algorithm-Study) | Python 문제풀이 | swjungle 수료 후
- swjungle
	- [[week 01~03 {swjungle} {ALGORITHMS}]]
	- [[week12 {swjungle}{ALGORITHMS}]]
