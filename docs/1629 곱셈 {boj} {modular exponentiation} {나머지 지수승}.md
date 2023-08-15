---
aliases: 
tags: 
description:
title: 1629 곱셈 {boj} {modular exponentiation} {나머지 지수승}
created: 2023-08-15T22:56:34
updated: 2023-08-15T23:05:05
---

- [[0011 Algorithms ♾️|algorithms]]  

나머지 지수승 관련한 이산수학 교재 캡처 추가. 이 알고리즘이 기반한 원리는 $b^4 \mod m$ 이나 $(b^2 \mod m) \cdot (b^2 \mod m) \mod m$ 이나 같다는 것이다. 주목할 변수는 `x`일지 몰라도 그 x에 붙일 계수를 알기 위해선 당연하게도 `power`를 지수승만큼 증가시켜야 한다. 그런데 그냥 $b^1$ $b^2$ $b^3$ ... 이런식이 아닌, $b^{2^1}$ $b^{2^2}$ $b^{2^3}$ 이런 식으로 증가하기 때문에 `power`를 매 이터레이션마다 제곱을 시키는 것이다.

![[software - 0.jpg]]
