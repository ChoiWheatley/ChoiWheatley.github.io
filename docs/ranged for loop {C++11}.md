---
aliases: 
tags: 
description:
title: ranged for loop {C++11}
created: 2024-01-16T19:30:59
updated: 2024-01-16T20:00:53
---
- [[C++]]
- [포프TV / 내가 쓰는 C++: Range Based For Loop](https://youtu.be/sVoz36DYK5s)
- [cppreference.com / Range-based for loop](https://en.cppreference.com/w/cpp/language/range-for)
---

## 문법

아래 두 for 문은 완전히 동일한 결과를 보여준다.

```cpp
std::vector<int> v = {1, 2, 3, 4, 5};
for (auto i = v.begin(); i != v.end(); ++i) {
	std::cout << *i << ",";
}
for (auto i : v) {
	std::cout << i << ",";
}
```

## 조건

`begin()`, `end()` 멤버함수를 구현한 타입, 컴파일 시간에 크기를 알 수 있는 배열은 range-based-for-loop을 사용가능하다.

## 동적할당된 array를 RBFL하려면?

## 임의의 타입을 RBFL 하려면?
