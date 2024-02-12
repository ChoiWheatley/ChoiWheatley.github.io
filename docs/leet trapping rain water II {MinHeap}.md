---
aliases: 
tags:
  - algo/heap
description: 
title: leet trapping rain water II {MinHeap}
created: 2024-02-12T16:37:54
updated: 2024-02-12T16:46:12
---
- <https://leetcode.com/problems/trapping-rain-water-ii>

## Pull Requests

<https://github.com/Jungle-7-Algorithm-study/Algorithm-Study/pull/91>

- \[leet-407] ❌ trapping-rain-water-ii

일차원적인 [trapping-rain-water-i](https://leetcode.com/problems/trapping-rain-water-i) 마냥 바깥에 무한대 height 크기의 벽이 있다고 가정하고 풀었더니 오답이 발생함. 물이 빠지는 것이 단순히 일차원만 보면 안되기 때문.

- \[leet-407] ⌛️ trapping-rain-water-ii

0층부터 DFS를 시도한다. 가장자리서부터 탐색을 시작, 가장자리와 연결된 모든 순회 가능한 지점들은 덧셈에 포함하지 않는다. ⟹ 최대높이가 $2 \times 10 ^ 4$ 이고 각 층마다 N^2 라서 타임아웃 발생

- \[leet-407] 🆗 trapping-rain-water-ii minheap으로 풀이

1. 가장자리는 언제나 물을 가둘 수 없기 때문에 MinHeap(이하 queue)에 추가한다.
2. queue에서 셀을 꺼낼 때마다 최소높이가 튀나오기 때문에 물을 가둘 수 있는 최소수위 level이 된다.  
	level은 감소하지 않는다.
3. 튀어나온 셀의 인접 셀을 비교했을 때 현재 level보다 낮은 셀을 발견한다면 그 셀은 물을 가둘 수  
	있게 된다. 그 차이만큼 ret에 추가한다. queue에 인접 셀들을 추가한다.
