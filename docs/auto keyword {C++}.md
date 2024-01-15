---
aliases: 
tags: 
description:
title: auto keyword {C++}
created: 2024-01-14T20:13:56
updated: 2024-01-15T20:35:56
---
- [[C++]]
- [포프TV / 내가 쓰는 C++11: auto](https://youtu.be/GmyXz-HQY-U)
- [modoocode.com / C++ STL - 벡터, 리스트, 데크](https://modoocode.com/223)
---

## auto의 세가지 얼굴

1. 일반 auto: 2번을 포함하는 타입
2. `auto *`: 포인터 변수의 타입을 추론할때 사용, 굳이 `*` 넣지 않아도 됨
3. `auto &`: 레퍼런스 변수의 타입을 추론할때 사용. `&`를 넣지 않으면 복사가 됨. 만약 복사 연산자 `operator=`가 delete 됐다면?

## auto로 광명찾은 iterator들

`std::vector<int>::iterator`라는 거대한 길이의 타입을 `auto`만으로 충분하게 되었다.

```cpp
for (const auto & elem : vec) {
	 std::cout  << elem << std::endl;
}
```

## auto functions

- [cppreference.com / function declaration](https://en.cppreference.com/w/cpp/language/function)

리턴타입을 파이썬처럼 뒤에다 쓸 수 있다.

```cpp
auto fun() -> int {
	return 1;
}
```

- C++14부터 리턴타입을 굳이 적지 않더라도 컴파일러가 return문을 확인하고 추론할 수 있다고 한다. return statement가 없으면 void

```cpp
int x = 1;
auto f() { return x; } // return type is int
const auto & f() { return x; } // return type is const int &
```

## Initializer_list와 auto

- [modoocode.com](https://modoocode.com/286#page-heading-3)

C++17 이전

```cpp
auto a = {1};     // std::initializer_list<int>
auto b{1};        // std::initializer_list<int>
auto c = {1, 2};  // std::initializer_list<int>
auto d{1, 2};     // std::initializer_list<int>
```

C++ 17 부터 아래와 같이 두 가지 형태로 구분해서 `auto` 타입이 추론됩니다.

- `auto x = {arg1, arg2...}` 형태의 경우 `arg1`, `arg2` ... 들이 모두 같은 타입이라면 `x` 는 `std::initializer_list<T>` 로 추론됩니다.
- `auto x {arg1, arg2, ...}` 형태의 경우 만일 인자가 단 1 개라면 인자의 타입으로 추론되고, 여러 개일 경우 오류를 발생시킵니다.

```cpp
auto a = {1};     // 첫 번째 형태이므로 std::initializer_list<int>
auto b{1};        // 두 번째 형태 이므로 그냥 int
auto c = {1, 2};  // 첫 번째 형태이므로 std::initializer_list<int>
auto d{1, 2};  // 두 번째 형태 인데 인자가 2 개 이상이므로 컴파일 오류
```
