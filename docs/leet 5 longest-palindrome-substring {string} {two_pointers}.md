---
aliases: 
tags:
  - algo/string
  - algo/two_pointers
description: 
title: leet 5 longest-palindrome-substring {string} {two_pointers}
created: 2024-01-02T17:43:53
updated: 2024-01-03T18:09:23
---
- [[0011 Algorithms ♾️|algorithms]]
- <https://leetcode.com/problems/longest-palindromic-substring>

## TL;DR

- 길이 2, 3짜리 윈도우를 슬라이딩 하며 매 위치마다 투 포인터를 좌우로 확장하는 식으로 팰린드롬을 검사하면 된다. => 이렇게 했을때의 시간복잡도는 $O(N^2)$
- 길이 length짜리 윈도우를 슬라이딩 하면서 투 포인터를 가운대로 수축하는 형태로 팰린드롬을 검사하는 나의 방식은 시간복잡도가 $O(N^3)$가 나온다.

## $O(N^2)$ 짜리 코드

```python

class Solution2:
    def longestPalindrome(self, s: str) -> str:
        """
        책의 설명에 따르면, 투 포인터가 중앙을 중심으로 확장하는 형태로 푼다.
        나머지는 슬라이딩 윈도우로 체크하고, 홀수로 출발하는 케이스와 짝수로 출발하는 케이스 모두 다해서 max를 구한다.

        🆗
        195ms 17.42MB

        왜 Solution1에 비해 빠르지? 둘 다 최악의 경우 N**2인 거 아닌가? 눈에 보이는 차이점이라곤 수축과 확장인데,
        """

        def expand(l: int, r: int) -> str:
            while l >= 0 and r < len(s) and s[l] == s[r]:
                l -= 1
                r += 1
            return s[l + 1 : r]

        if len(s) < 2 or s == s[::-1]:
            return s

        result = ""
        for i in range(len(s) - 1):
            result = max(result, expand(i, i + 1), expand(i, i + 2), key=len)

        return result
```

## $O(N^3)$짜리 코드

```python

class Solution1_1:
    def longestPalindrome(self, s: str) -> str:
        """
        🆗
        2931ms 17.38MB
        바뀐거: 
        - palindrome 검사할때 문자열 슬라이스가 아니라 인덱스만 넘겨줬다. 
        - 원래부터 palindrome인 문자열은 그대로 리턴하는 코드가 추가됐다

        Solution2와의 차이점은 윈도우의 크기를 2/3에서 시작하느냐, length에서 시작하느냐에 있다. 윈도우의 크기를 확장하는 방향은 
        중복을 없애고 이전 결과를 그대로 활용할 수 있다.

        하지만 Solution1과 1_1의 문제풀이는 이전 결과를 활용하지 못한다. 윈도우의 length를 먼저 정하고 슬라이딩 하면서 각각에 대하여
        검사하는 방법은 같은 인덱스의 중심을 가진 부분문자열을 여러번 다시 계산해야 하기 때문에 시간복잡도에 N을 더 곱하게 된다.

        두 방식의 팰린드롬 검사시간은 O(N)으로 동일하지만 전체적인 복잡도를 보면 Solution 1과 1_1은 O(N**3)이 소요됐고, Solution2는
        O(N**2)가 소요돼 여기에서 시간차가 발생한 것이다.
        """

        def is_palindrome(l: int, r: int) -> bool:
            while l < r:
                if s[l] != s[r]:
                    return False
                l += 1
                r -= 1
            return True

        if len(s) < 2 or s == s[::-1]:
            return s

        for length in range(len(s), 1, -1):
            for start in range(len(s) - length + 1):
                end = start + length
                if is_palindrome(start, end - 1):
                    return s[start:end]

        return s[0]
```
