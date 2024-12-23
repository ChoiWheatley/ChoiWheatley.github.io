---
links:
status:
description: 
title: No. 24 - 보급로
created: 2023-02-10T21:37:08
categories:
  - heap
  - bfs
  - memoization
aliases:
  - 보급로
tags:
  - heap
date created: Friday, February 10th 2023, 9:37:08 pm
date modified: Monday, February 27th 2023, 6:20:45 pm
updated: 2024-12-23T17:58:36
---
{% raw %}

parent link: [[Heap]] [[SW Expert Academy|swea]]

---

[No. 24 [S/W 문제해결 응용] 4일차 - 보급로](https://swexpertacademy.com/main/talk/codeBattle/problemDetail.do?contestProbId=AV15QRX6APsCFAYD&categoryId=AYWab_JKjkwDFAQK&categoryType=BATTLE&battleMainPageIndex=1)

priority_queue로는 못 풀었고, BFS + memoization으로 풀었다. 일반 queue를 사용하여 visited를 대체한 `g_costs` 변수를 활용하여 push 조건을 걸었다. 이러면 비록 다녀간 노드일지라도 더 효율적인 방법이 존재하면 갱신하고 push하는 식으로 작동한다.

```cpp
static int N = 0;
/**visited를 대신하는 전역변수. [i][j]보다 작은 경우 pq에 넣는다.*/
static Arr<Arr<int>> g_costs{{{MAX_INT}}};
/**말 그대로 input데이터*/
static Arr<Arr<int>> g_data;
constexpr auto four_ways = {
    idx_t(-1, 0), // up
    idx_t(0, 1),  // right
    idx_t(1, 0),  // down
    idx_t(0, -1), // left
};

inline int solution(int n, array<array<int, MAX_N>, MAX_N> &&arr2d) {
  init(n, std::move(arr2d));
  auto q = queue<idx_t>();

  q.push(idx_t{0, 0});

  while (!q.empty()) {

    idx_t cur = q.front();
    q.pop();
    for (idx_t delta : four_ways) {
      if (out_of_bounds(delta + cur, n)) {
        continue;
      }
      auto next = delta + cur;
      int next_cost = get(g_costs, cur) + get(g_data, next);
      // not visited OR 더 적은 비용으로 갈 수 있는 경우
      if (get(g_costs, next) > next_cost) {
        get(g_costs, next) = next_cost;
        q.push(next);
      }
    }
  }

  return g_costs[n - 1][n - 1];
}
```

priority queue(max heap)를 사용하면 50ms 정도의 시간으로 풀 수 있을 것 같은데... 어떻게 풀어야 할지 잘 모르겠다.


{% endraw %}
