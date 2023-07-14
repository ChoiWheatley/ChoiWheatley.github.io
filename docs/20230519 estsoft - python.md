---
description:
aliases: 
tags: 
created: 2023-05-19T09:15:58
updated: 2023-07-11T15:21:10
title: 20230519 estsoft - python
---
# 페이지 교체 알고리즘

## MRU (Most Recently Used)

MISS => `pop(0)`를 수행함.
HIT가 되면 `pop(0)`하지 않음.

## LRU (Least Recently Used)

MISS => `pop(0)`를 수행함.
HIT => 히트한 원소를 꺼내어(위치상관없이) 다시 삽입한다.

deque을 사용하면 `maxlen`을 설정하여 자동으로 `popleft`를 수행하게 만들 수 있군.

## FIFO (First In First Out) 

선입선출 알고리즘
무지성으로 queue를 사용하면 이렇게 된다. 당연히 출제빈도는 낮음


# Sliding Window and Two Pointer

- two pointer => 두 개의 점의 위치를 사용
- sliding window => 슬라이싱을 사용하여 고정된 길이의 윈도우를 움직이며 순회

구간합, 회문

- [ ] 슬라이딩 윈도우 : [2, 5, 4, 6, 4, 6, 7, 6, 4, 3, 1, 2]에서 연속된 2개의 합이 10인 모든 수를 찾아라.

# Recursion

## Palindrome with stack | queue | recursion 


