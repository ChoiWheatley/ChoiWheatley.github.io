---
aliases: 
tags: 
description:
title: Union Find
created: 2023-08-20T23:40:36
updated: 2023-08-24T12:25:23
---
[[1197 최소 스패닝 트리 {boj}]]  
[kruskal algorithm explaning blog](https://chanhuiseok.github.io/posts/algo-33/)

connected component를 찾는데 사용된다. 같은 connected component에 있다면 서로 연결되어 있다는 것을 의미한다. DFS, BFS를 사용해서 탐색을 수행하는 방법도 있겠지만 Union Find를 사용하면 같은 Set 여부만 파악하면 되므로 상대적으로 빠르다.

union find란, 서로소 집합(공통분모가 없는 두 집합)을 표현하는 자료구조. 최소 스패닝 트리 문제를 풀 때 크루스칼 알고리즘을 사용하였다. 이때 사이클을 탐지해야 하는 상황에서 Union Find라는 기법을 알게 되었다. 얘는 두개의 서로소 집합을 Union, 즉 합집합 연산을 하는 것으로 간선을 추가해 갈 수 있다. 

그러면 Union Find는 어떤거냐? 간선을 추가해 나갈때마다 간선의 정점이 기존의 집합에 속하는지 검색해야 하는데, 이를 두고 말하는 것이다.

[서로소 집합을 만들어나가는 과정](https://m.blog.naver.com/ndb796/221230967614)

```python
def get_parent(disjoint_set: List[int], idx: int) -> int:
    if disjoint_set[idx] == idx:
        # 자기자신이 루트(서로소집합 그 자체)인 경우
        return idx
    return get_parent(disjoint_set, disjoint_set[idx])


def union_parent(disjoint_set: List[int], a, b):
    """두 서로소 집합을 합친다. 만약 서로소 집합이 아닌 경우 에러를 발생시킨다."""
    set_a = get_parent(disjoint_set, a)
    set_b = get_parent(disjoint_set, b)
    if set_a == set_b:
        raise NotADisJointSet()
    if set_a > set_b:
        # 작은 인덱스가 부모가 되도록
        set_a, set_b = set_b, set_a
    disjoint_set[set_b] = set_a
```

## Union Find의 시간복잡도는?
