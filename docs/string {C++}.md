---
aliases: 
tags: 
description:
title: string {C++}
created: 2024-01-13T12:26:28
updated: 2024-01-13T17:02:04
---
- [[C++]]
- [[커스텀 타입을 string으로 변환하는 방법 {C++}]]
- [모두의코드 10-4. C++ 문자열의 모든 것 (string과 string_view)](https://modoocode.com/292)
- [std\:\:basic_string](https://en.cppreference.com/w/cpp/string/basic_string) 문자열을 저장하고 수정하는 기능을 정의해놓은 템플릿 클래스 
- [std\:\:char_traits](https://en.cppreference.com/w/cpp/string/char_traits) 문자(열) 연산들에 대한 기본적인 기능을 정의해놓은 템플릿 클래스. 
	- 대소비교, 할당/이동/복사 연산자, find, 변환 등을 정의한다.
- [[string_view {C++}]]
- 

> basic_string과 char_traits의 차이점을 설명해주세요

`basic_string`은 문자열 데이터가 저장하는 C 스타일 배열을 조작하는 기능()

> Short String Optimization(SSO)에 대해서 설명해주세요

- https://modoocode.com/292#page-heading-2
- [[overload operator new {C++}]]

basic_string이 저장하는 문자열의 길이는 특정할 수는 없지만 많은 경우 길이가 짧은 문자열을 생성하고 소멸합니다. 이 경우 매번 동적할당과 free를 반복하게 되면 굉장히 비효율적입니다. 따라서 길이가 작은 문자열을 생성하게 될 경우 객체 멤버에 저장해 버리는 최적화를 수행합니다.

하지만 모든 라이브러리가 SSO를 하는 것은 아니며, 이는 구현체에 따라 달라질 수 있어 주의를 요하는 부분입니다.
