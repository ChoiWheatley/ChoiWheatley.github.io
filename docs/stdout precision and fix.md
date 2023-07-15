---
description:
aliases: 
tags: 
created: 2023-04-12T18:49:31
updated: 2023-07-15T21:33:03
title: stdout precision and fix
---
- [boj 17073 나무 위의 빗물](boj.kr/17073) 을 풀다가 다른 유저의 [질문](https://www.acmicpc.net/board/view/37116)을 통해 알게되었다.
- [ref](https://cplusplus.com/reference/ios/fixed/) 에 따르면 부동소수점 표현방식에 대하여 precision, 정밀도를 설정할 수 있고, 변수에 따라 유동적인 플래그 값을 fix할 수 있다고 한다. 
- `std::fixed` 플래그는 고정소수점 표현방식으로만 표현할 수 있다.
- `std::scientific` 플래그는 과학적 표현방식 (10e-1) 으로 표현한다.
- `std::hexfloat` 플래그는 16진수 소수점으로 표현한다.
- 기본적으로, `std::defaultfloat` 플래그가 활성화 되어있으며, 일정 기준치를 넘으면 자동으로 `std::scientific`으로 전환되는 건가보다. (정확히 몇자리부터 바뀌는지 안나옴)

```cpp
cout.precision(6); // 소수 6자리까지 표현
cout << std::fixed << // 무조건 고정소수점으로만 표현
	answer << "\n;
```
