---
aliases: 
tags: 
description: 
links:
  - "[[0011 Algorithms ♾️|algorithms]]"
status: 
title: itertools combinations 분석
created: 2025-01-02T14:09:18
updated: 2025-01-02T14:09:52
---

```python
# from itertools import combinations

def combinations(iterable, r):
    # combinations('ABCD', 2) → AB AC AD BC BD CD
    # combinations(range(4), 3) → 012 013 023 123

    pool = tuple(iterable)
    n = len(pool)
    if r > n:
        return
    indices = list(range(r))

    yield tuple(pool[i] for i in indices)
    while True:
        for i in reversed(range(r)):
            if indices[i] != i + n - r:
                break
        else:
            return
        indices[i] += 1
        for j in range(i+1, r):
            indices[j] = indices[j-1] + 1
        yield tuple(pool[i] for i in indices)
```

`@local indices`: 크기 r 짜리 인덱스, 얘를 섞음으로써 combination을 만들어낸다.  
`@local pool`: iterable 인자를 보관함.

[indices 상태 시뮬레이션]

n = 5, r = 3이라고 가정  
yield 초기 indices 그대로 리턴 

```
-> (0,1,2)
```

반복문 안에서 for else 구문 실행, else는 break를 만나지 않았을 경우 실행된다.  
break 조건은 `indices[i] != i + n - r`, 현재 조건에 따르면 `indices[i] != i + 2`인 경우 활성화 되는데, 이것은 indices의 각 원소가 pool의 인덱스를 넘어서지 않기 위한 조건으로 보인다. i는 reversed로 돌아가니까 가장 마지막 원소부터 증가시키므로, 통상적인 combination 수열을 만드는 방식과 동일하다.

```
-> (0,1,3) -> (0,1,4) 
		+          +
```

break가 되면, `indices[i] += 1` 하는 것과 함께 `j=i+1..r indices[j]`를 이전 원소 + 1만큼으로 설정한다. 중복되는 index가 있으면 안되고 뒤 인덱스가 i+n-r일 경우 초기화 하기 위한 용도로도 쓰인다.

```
-> (0,2,3) -> (0,2,4) -> (1,2,3) -> (1,2,4) -> (1,3,4) -> (2,3,4)
	  + ^          +      + ^ ^          +        + ^      + ^ ^
```
