---
aliases: 
tags: 
description:
title: 2629 양팔저울 {boj} {dp, DFS}
created: 2023-08-28T11:05:59
updated: 2023-08-28T11:20:09
---
2차원 DP를 사용하여 풀 거라고는 생각조차 하지 못해 너무 어려운 문제였다. 우리 동전문제 풀 때에도 비슷하게 추를 *0부터 index까지* 사용했을때의 결과를 배열에 저장하는 것이었다.

> `dp[idx][weight]` = 0~idx번째 추를 사용하여 weight 만큼의 무게를 잴 수 있는가?

게다가 내가 참고한 [블로그 글](https://yabmoons.tistory.com/105)은 index가 아니라 count 인자를 사용했고, 매 이터레이션마다 `visited`를 사용하는 방식이 이해가 전혀 되지 않았다. 재귀를 호출할 때 `Make_Weight(Cnt + 1, Result + Weight[Cnt])` 를 하는데, 이거, 호출할때마다 `Cnt+1`과 `Weight[Cnt]` 가 참조하고자 하는 인덱스가 1씩 차이가 나게 되더라. 그래서 코드를 보고 쳐도 계속 안됐던 거고, 이해를 못했던 것이다. 따라서 내가 이해한 대로 문제를 다시 해석해 재귀함수를 만들었다.

```python
def r_sol(N: int, idx: int, weight: int):
    """
    0번째부터 idx번째 추 까지 사용하여 weight를 만들 수 있는가?
    """
    if g_visited[idx][weight]:
        return

    g_visited[idx][weight] = True

    if not (idx < N):
        return

    r_sol(N, idx + 1, weight + W[idx + 1])  # 추에 무게를 더하는 경우
    r_sol(N, idx + 1, weight)  # pass
    r_sol(N, idx + 1, abs(weight - W[idx + 1]))  # 반대쪽에 추를 다는 경우

# 추의 무게들 (sorted)
W: list[int]
# visited[idx][weight]: idx번째 추 까지 사용하여 weight를 만들 수 있는가?
g_visited: list[list[bool]]
```

`W`의 크기가 중요하다. 정확히 입력받은 `n`개만큼의 크기로 재귀를 돌리게 되면 `idx == N - 1`인 순간에서 `W[N]`을 참조하는 순간이 오기 때문이다. 따라서 `W` 입력받을 때 뒤에 0을 하나 더 붙이던가, 아예 `MAX_N`같은걸로 고정된 길이의 `W` 배열을 만드는 것이 정신건강에 더 좋을 듯 하다.
