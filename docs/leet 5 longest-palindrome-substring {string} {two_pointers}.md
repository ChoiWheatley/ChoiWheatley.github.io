---
aliases: 
tags:
  - algo/string
  - algo/two_pointers
description: 
title: leet 5 longest-palindrome-substring {string} {two_pointers}
created: 2024-01-02T17:43:53
updated: 2024-01-02T17:49:46
---
- [[0011 Algorithms ♾️|algorithms]]
- <https://leetcode.com/problems/longest-palindromic-substring>

## TL;DR

- 길이 2/3짜리 윈도우를 슬라이딩 하며 매 위치마다 투 포인터를 좌우로 확장하는 식으로 팰린드롬을 검사하면 된다. => 이렇게 했을때의 시간복잡도는 $O(N^2)$
- 길이 length짜리 윈도우를 슬라이딩 하면서 투 포인터를 가운대로 수축하는 형태로 팰린드롬을 검사하는 나의 방식은 시간복잡도가 $O(N^3)$가 나온다.
