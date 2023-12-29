---
description:
aliases: 
tags: algo/dp 
date created: Monday, February 13th 2023, 6:16:28 am
date modified: Monday, February 27th 2023, 6:20:45 pm
created: 2023-02-13T06:16:28
updated: 2023-08-28T19:49:46
title: LCS 가장 긴 공통 부분수열 {longest common subsequence}
---

# LCS 가장 긴 공통 부분수열

상태: 풀이완료  
태그: dp, string

[최장공통부분수열 - swea](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AWBOHEx66kIDFAWr) | [wiki](https://ko.m.wikipedia.org/wiki/최장_공통_부분_수열) | [9251 LCS {boj}](https://www.acmicpc.net/problem/9251) | [누워서 보는 알고리즘 {YT}](https://youtu.be/z8KVLz9BFIo?feature=shared)

# 개요

Longest Common Sequence 

두 문자열이 주어졌을 때 —문자열도 결국 수열이니까— 두 문자열의 공통 부분수열 중 가장 긴 녀석의 길이를 찾아라. *최적화 문제이구나*

예를 들어 "acaykp"와 "capcak"의 경우, 두 문자열의 최대 공통 부분 수열은 "acak"로 길이가 4이다. 예시와 같이 공통부분”문자열”이 아니라 공통부분”수열”이라는 점 유의하고.

# 3중 for 문을 활용한 나의 방식

- memo[i] = s1의 i번째 인덱스까지의 LCS의 길이
- begin[i] = s2의 탐색을 시작할 인덱스

```python
def sol(s1, s2):
	memo_prev = 0
	for start = 0..<len(s1):
		for i = start..<len(s1):
			memo[i] = memo_prev
			for j = begin[i]..<len(s2):
				if s1[i] == s2[j]:
					begin[i+1] = j + 1
					memo[i] += 1
					memo_prev = memo[i]
					break
```

결과 = s1의 길이를 N, s2의 길이를 M이라고 했을 때 무려 $O(N^2M)$의 숫자가 나와버렸다. 두 문자열의 최대 길이는 1000이므로 1000^3 = 1’000’000’000이니 1초안에 못푼다. 따라서 다른 방법을 구해야 한다.

# 접두어를 부분문제로 두고 푸는 위키의 설명

- LCS(X,Y) = X와 Y의 LCS
- Xi = X의 i개의 접두어
- Yi = Y의 i개의 접두어
- xi = X의 i번째 문자
- yi = Y의 i번째 문자

가장 작은 접두어는 당연히 빈 수열일테니, i = 0 or j = 0일때다. 따라서

$$
\text{LCS}(X_0,Y_i)_{i\le size(Y)}=~~\\ \text{LCS}(X_i,Y_0)_{i\le size(X)}
$$

부분문제 : 

1. 두 수열 Xn과 Ym이 같은 원소로 끝나는 경우
    
    마지막 원소 xn과 ym을 제거할 수 있다. 그리고 두 짧아진 문자열 Xn-1, Ym-1에 대한 LCS를 찾고 삭제한 원소를 뒤에 붙여주면 된다.

    $$
    \text{LCS}(X_n, Y_m) = \text{LCS}(X_{n-1},Y_{m-1}),x_n
    $$

2. 두 수열 Xn과 Ym이 같은 원소로 끝나지 않는 경우 다음과 같은 경우 중 하나에 해당하게 될 것이다.
    1. LCS(Xn, Ym)이 xn으로 끝나는 경우
    2. LCS(Xn, Ym)이 xn으로 끝나지 않는 경우
    
    a의 사례는 xn을 지워버리면 전체 LCS에 손실이 발생할 테지만 ym을 지운다고 손실이 일어나지 않는다. 따라서 $\text{LCS}(X_n, Y_{m-1})$
    
    b의 사례는 반대로 xn을 지운다고 손실이 일어나지 않는다. 따라서$\text{LCS}(X_{n-1}, Y_{m})$두
    
    2번 사례는 a와 b 경우밖에 존재하지 않기 때문에 둘 중에 더 긴 녀석을 선택하기만 하면 된다. 따라서

$$
\text{LCS}(X_n, Y_{m})= \begin{cases}
	\text{longest}(\text{LCS}(X_n, Y_{m-1}),~~\text{LCS}(X_{n-1}, Y_{m})) ~\text{if}~x_n \ne y_m  \\
	\text{LCS}(X_{n-1}, Y_{m-1})+x_n ~\text{if}~x_n = y_n
\end{cases} 
$$

### Recurrence Relation

재귀적 관계로 나타낸 수도코드는 다음과 같다.

```python
def LCS(X: str, Y: str) -> int:
    """X와 Y의 최장 공통부분수열의 길이를 구한다."""
    if len(X) == 0 or len(Y) == 0:
        return 0

    if X[-1] == Y[-1]:
        # 두 접두어가 같은 문자로 끝난다 => LCS의 멤버임이 확실
        return LCS(X[:-1], Y[:-1]) + 1
    # 두 접두어가 다른 문자로 끝난다. xn을 살리는 쪽과 yn을 살리는 쪽
    # 둘 중에 더 긴 녀석을 선택한다.
    return max(LCS(X[:-1], Y), LCS(X, Y[:-1]))
```

### Bottom-Up Style with memoization

아무래도 중복 부분문제 (Overlapping Subproblems)들이 자주 겹치다 보니 시간초과가 발생하게 되었다. 심지어 `@lru_cache(256)`, `@cache` 데코레이터를 사용하여도 각각 시간초과, 메모리 초과 에러가 발생하였다. 따라서 고전적인 방식 그대로 코드를 재작성했다. X와 Y 앞에 빈 공백 문자를 넣는다는게 바로 와닿지 않았다. 하지만 초기값을 지정하기 위한 방법이었던 것이고, 원치 않는다면 그냥 DP 공간을 1씩 넓힌 다음 X, Y 인덱싱 할 때 문자열 참조 인덱스를 1씩 줄이면 된다.

```python
def LCS(X: str, Y: str) -> int:
    """X와 Y의 최장 공통부분수열의 길이를 구한다."""
    # ∵ 빈 수열도 비교해야 하기 때문. (초기조건)
    X = " " + X
    Y = " " + Y
    dp = [[0 for _ in range(len(Y))] for _ in range(len(X))]
    for i in range(1, len(X)):
        for j in range(1, len(Y)):
            if X[i] == Y[j]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    return dp[-1][-1]

```

### 예제

X = ACAYKP  
Y = CAPCAK  
라고 할 때, 상향식 방식으로 DP 배열을 만들고자 하는 경우 다음과 같이 그릴 수 있다.  
![[IMG_2714.jpg]]

## LCS 수열 자체도 찾고싶다면?

우리가 사용한 dp와 동일한 크기의 이차원 배열 `dp_b`를 만들어 그 안에 방향정보를 넣어주면 된다. 예를 들어 두 접두어가 같은 문자로 끝나는 경우에는 대각선으로 이동하기 때문에 `Dir.SE`를, 두 접두어가 다르게 끝날 경우엔 더 긴 쪽으로 가는 것이 유리하므로 각각 `Dir.E` 또는 `Dir.S`라는 반복자를 넣어 관리했다.

그 결과로, `len(X)-1, len(Y)-1` 위치에서부터 반복자를 거꾸로 트레이싱 하면서 두 접두어가 일치하는 경우에만 결과값에 추가해주면 뒤집어진 LCS 수열을 찾을 수 있게 된다. 다음은 [9252 LCS 2 {boj}](https://boj.kr/9252) 문제를 풀었을 때의 소스코드이다.

```python
from enum import Enum, auto
from typing import Callable


class Dir(Enum):
    SE = auto()
    E = auto()
    S = auto()


def LCS(X: str, Y: str, hook: Callable[[int, int, Dir], None]) -> int:
    """X와 Y의 최장 공통부분수열의 길이를 구한다."""
    # ∵ 빈 수열도 비교해야 하기 때문. (초기조건)
    dp = [[0 for _ in range(len(Y))] for _ in range(len(X))]
    for i in range(1, len(X)):
        for j in range(1, len(Y)):
            if X[i] == Y[j]:
                dp[i][j] = dp[i - 1][j - 1] + 1
                hook(i, j, Dir.SE)
            elif dp[i - 1][j] > dp[i][j - 1]:
                hook(i, j, Dir.S)
                dp[i][j] = dp[i - 1][j]
            else:
                hook(i, j, Dir.E)
                dp[i][j] = dp[i][j - 1]
    return dp[-1][-1]


class Tracer:
    """공통부분수열의 문자열을 찾기 위해 만든 클래스"""

    dp: list[list[Dir]]

    def __init__(self, xlen: int, ylen: int):
        self.dp = [[Dir.E for _ in range(ylen)] for _ in range(xlen)]

    def hook(self, i: int, j: int, d: Dir):
        self.dp[i][j] = d


if __name__ == "__main__":
    X = input()
    Y = input()
    X = " " + X
    Y = " " + Y
    tracer = Tracer(len(X), len(Y))
    lcs_len = LCS(X, Y, tracer.hook)
    print(lcs_len)

    if lcs_len > 0:
        # tracer 안에 있는 정보를 역으로 추적해가며 문자열을 생성한다.
        result = ""
        x = len(X) - 1
        y = len(Y) - 1
        while x > 0 and y > 0:
            match tracer.dp[x][y]:
                case Dir.E:
                    y -= 1
                case Dir.S:
                    x -= 1
                case Dir.SE:
                    result += X[x]
                    x -= 1
                    y -= 1
        print(result[::-1])

```
