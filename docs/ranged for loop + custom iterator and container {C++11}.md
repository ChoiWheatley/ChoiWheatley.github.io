---
aliases: 
tags: 
description:
title: ranged for loop + custom iterator and container {C++11}
created: 2024-01-16T19:30:59
updated: 2024-01-186T2207:12:24
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

또한, 정적배열도 rbfl 돌릴 수 있다.

```cpp
int array[] = {1, 2, 3, 4, 5};
for (auto i : array) {
	std::cout << i << " ";
}
```

## 조건

- iterator-like 객체를 리턴하는 `X::begin()`, `X::end()` 멤버함수를 구현한 타입, 컴파일 시간에 크기를 알 수 있는 배열은 range-based-for-loop 사용가능하다.
- Free Function `begin(X&)`, `end(X&)`를 구현한 경우 또한 사용가능하다.
- iterator-like란, 다음 연산을 지원하는 객체를 의미한다.
	- `operator++` => advance 연산
	- `operator!=` => `end`와의 비교연산
	- `operator*` => 역참조 연산

## 실습: 나만의 컨테이너

```cpp
#include <iostream>

/**
 * @brief 커스텀 컨테이너, 근데 RBFL를 가능하게 만든.
 *
 */
class MyContainer {
    int *mArr;
    size_t mSize;

  public:
    class iterator {
        int *mCur;

      public:
        explicit iterator(int *position) : mCur{position} {}

        /// @brief 전위 증가연산자
        iterator &operator++() {
            mCur++;
            return *this;
        }

        /// @brief end 여부와 비교하기 위한 연산자
        bool operator!=(const iterator &rhs) { return mCur != rhs.mCur; }

        int &operator*() { return *mCur; }
    };
    MyContainer(size_t size) : mArr{new int[size]}, mSize{size} {}

    iterator begin() { return iterator{mArr}; }
    iterator end() { return iterator{mArr + mSize}; }

    int &operator[](size_t idx) { return mArr[idx]; }
};

int main(void) {
    std::cout
        << R"(1. 아래는 정적크기 배열을 RBFL(range-based-for-loop)에 넣었을 때
    정상작동하는 모습을 보여준다.
)";
    int array[] = {1, 2, 3, 4, 5};
    for (auto i : array) {
        std::cout << i << " ";
    }
    std::cout << std::endl;

    std::cout
        << R"(2. 아래는 커스텀 컨테이너 MyContainer를 만들어서 RBFL을 돌렸을 때
    정상작동하는 모습을 보여준다.
)";
    MyContainer container{5};
    for (size_t i = 0; i < 5; ++i) {
        container[i] = i * i;
    }
    for (auto itr = container.begin(); itr != container.end(); ++itr) {
        std::cout << *itr << " ";
    }
    std::cout << std::endl;
    for (auto i : container) {
        std::cout << i << " ";
    }
    std::cout << std::endl;

    return 0;
}
```
