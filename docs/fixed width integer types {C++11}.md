---
aliases: 
tags: 
description:
title: fixed width integer types {C++11}
created: 2024-01-15T20:39:45
updated: 2024-01-15T21:41:4400
---
- [[C++]]
- [cppreference.com / fixed width integer types](https://en.cppreference.com/w/cpp/types/integer)
- [learncpp.com / fixed width integers and size_t](https://www.learncpp.com/cpp-tutorial/fixed-width-integers-and-size-t/)
---

## TL;DR

- 정수형의 크기를 상관할 필요가 없을땐 `int`를 써라. 그게 제일 빠르다.
- 정수의 범위를 보장해야 할 땐 `std::int#_t`를 써라.
- 비트연산을 할땐 `std::uint#_t`를 써라.
- `short`, `long` 사용을 피하라. 대신 fixed-width 정수형을 사용하라.
- 정수를 저장하려고 unsigned 타입을 사용하지 말라.
- fast, least 타입 사용을 피하라. (그것이 C++11에 추가됐을지라도)
- 컴파일러가 정의한 정수형 사용도 피하라. 예를 들어 MSVC가 정의한 `__int8`, `__int16`따위를 말한다.

## Fixed Width Integer

`<cstdint>`에 정의돼 있다.

- `std::int8_t`
- `std::uint8_t`
- `std::int16_t`
- `std::uint16_t`
- `std::int32_t`
- `std::uint32_t`
- `std::int64_t`
- `std::uint64_t`

## Downsides

1. 고정크기를 보장할 수 없는 아키텍처가 있다. 임베디드 아키텍처의 경우?
2. 아키텍처가 정의한 wider type (`size_t`) 연산보다 느려질 수 있다. 64비트 운영체제에서는 32비트 정수형보다 64비트 정수형을 더 빨리 처리할 수 있다.

## Fast & Least Integers

1. 1번 Downside를 해결하기 위해 나온 least integer `std::[u]int_least#_t`는 해당 아키텍처가 지원할 수 있는 적어도 `#` 보다는 큰 최소크기의 바이트 수를 리턴한다. (??)
2. 2번 Downside를 해결하기 위해 나온 fast integer `std::[u]int_fast#_t`는 해당 아키텍처가 더 빠르게 연산할 수 있는 적어도 `#` 바이트보다는 큰 바이트 수를 리턴한다.

## Downsides

- 일단 잘 안씀. 에러발생의 소지가 많음
- 2번 Downside를 해결하느라 메모리를 낭비할 수도 있음.

## 결론

least, fast integer 쓰지마셈 (...)

## size_t

1. size_t는 프로그램의 주소폭 (address width)와 일치한다.
2. 아키텍처의 primitive type의 크기는 항상 size_t의 크기보다 작거나 같다.
3. 모든 객체의 크기는 size_t가 표현할 수 있는 값보다 항상 작다.
