---
description:
title: Segment Tree 연습문제 1
created: 2023-02-14T23:48:22
categories: 
 - problem
 - 문제
aliases: 
 - 
tags: [" algo/segment_tree  ", algo/segment_tree]
state: "Pass"
date created: Tuesday, February 14th 2023, 11:48:22 pm
date modified: Monday, February 27th 2023, 6:20:45 pm
updated: 2023-07-11T15:21:07
---
parent link: [[Segment Tree]]

---

길이가 n인 수열 $a_0, a_1, \cdots, a_{n-1}$ $(0 ≤ a_i ≤ 10^9)$ 에서 아래 두 가지 쿼리를 처리하는 프로그램을 작성하라

  
•  `0 i x`    :    ai 를 x로 바꾼다. ($0 ≤ i ≤ n - 1, 0 ≤ x ≤ 10^9$)

•  `1 l r`    :    $\max(a_l, a_{l+1}, \cdots , a_{r-1}) - \min(a_l, a_{l+1}, \cdots , a_{r-1})$를 출력한다. (0 ≤ l < r ≤ n)  
 

**[입력]**  
첫 번째 줄에 테스트 케이스의 수 T 가 주어진다.

각 테스트 케이스의 첫 번째 줄에는 배열의 길이 n(1 ≤ n ≤ $10^5$)과 쿼리의 개수 q(1 ≤ q ≤ $10^5$)가 주어진다.  
두 번째 줄에는 배열 a가 주어진다.  
세 번째 줄부터 q개 줄에 걸쳐 쿼리가 주어진다.  
 

**[출력]**  
각 테스트 케이스마다 1번 쿼리의 결과를 공백으로 구분하여 출력한다.

입력
```
2  
5 5  
1 2 3 4 5  
1 0 5  
1 1 4  
0 2 9  
1 0 5  
1 1 4  
3 4  
0 5 10  
1 0 3  
0 0 5  
0 2 5  
1 0 3
```

출력
```
#1 4 2 8 7  
#2 10 0
```
