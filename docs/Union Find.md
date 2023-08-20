---
aliases: 
tags: 
description:
title: Union Find
created: 2023-08-20T23:40:36
updated: 2023-08-21T00:18:52
---
[[1197 최소 스패닝 트리 {boj}]]  
[kruskal algorithm explaning blog](https://chanhuiseok.github.io/posts/algo-33/)

union find란, 서로소 집합(공통분모가 없는 두 집합)을 표현하는 자료구조. 최소 스패닝 트리 문제를 풀 때 크루스칼 알고리즘을 사용하였다. 이때 사이클을 탐지해야 하는 상황에서 Union Find라는 기법을 알게 되었다. 얘는 두개의 서로소 집합을 Union, 즉 합집합 연산을 하는 것으로 간선을 추가해 갈 수 있다. 

그러면 Union Find는 어떤거냐? 간선을 추가해 나갈때마다 간선의 정점이 기존의 집합에 속하는지 검색해야 하는데, 이를 두고 말하는 것이다.

[서로소 집합을 만들어나가는 과정](https://m.blog.naver.com/ndb796/221230967614)
