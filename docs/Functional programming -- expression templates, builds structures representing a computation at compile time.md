---
description:
aliases: 
tags: 
created: 2023-04-12T22:10:18
updated: 2023-07-11T15:21:08
title: Functional programming -- expression templates, builds structures representing a computation at compile time
---
- https://en.wikipedia.org/wiki/Expression_templates
- functional programming - lazy eval과 대수적(algebraic) 접근, visitor를 사용한 패턴매칭, variant based ADT's, 불변 데이터타입, Concepts, typeerasure,
- [what can c++ offer as functional programming](https://stackoverflow.com/questions/21471836/what-can-c-offer-as-far-as-functional-programming)
- https://www.codeproject.com/Articles/1267996/Functional-Programming-in-Cplusplus

- 도서 `Functional Programming in C++ - Ivan Cukic` 추천서?
- https://medium.com/tech-book-reviews/functional-programming-in-c-4be0c359f61a

- 도서 `Effective STL - Scott Meyers` 위의 블로거가 함께 추천한 책임 
- https://medium.com/tech-book-reviews/effective-stl-90c2bebed852


bing chat의 요약
---
> Expression templates are a C++ template metaprogramming technique that builds structures representing a computation at compile time, where expressions are evaluated only as needed to produce efficient code. They were invented by Todd Veldhuizen and David Vandevoorde and are a popular technique for the implementation of linear algebra software, such as Dlib, Eigen, and Stan. Expression templates allow programmers to bypass the normal order of evaluation of the C++ language and achieve optimizations such as loop fusion. They use expression trees that only exist at compile time and can be constructed from any expression type.
> 
> Expression templates는 C++ 템플릿 메타프로그래밍 기술로, 컴파일 타임에 계산을 나타내는 구조체를 빌드합니다. 이 때 표현식은 전체 계산에 대한 효율적인 코드를 생성하기 위해 필요할 때만 평가됩니다. 따라서 Expression templates는 C++ 언어의 일반적인 평가 순서를 우회하고 루프 퓨전과 같은 최적화를 달성할 수 있습니다. Expression templates는 Todd Veldhuizen과 David Vandevoorde에 의해 독립적으로 발명되었으며, 선형 대수 소프트웨어의 구현에 많이 사용됩니다.[1][0]
> 
> Expression templates는 지연 평가를 구현하여 이러한 문제를 해결하며, 연산자 +가 두 개의 Vecs의 합을 나타내는 사용자 정의 유형 VecSum의 개체를 반환하도록하는 것으로 C++에서 구현할 수 있습니다. 그러나 이것은 자체적으로 비용이 많이 드는 평가를 수행하는 트리를 탐색하기 때문에 비용이 많이 듭니다. Expression templates는 컴파일 타임에만 존재하는 표현식 트리를 사용하여 지연 평가를 구현합니다. 따라서 필요한 경우 템플릿 인스턴스화에 의해 새로운 Vec 생성자가 생성됩니다. 이 생성자는 세 개의 Vec에서 작동하며 필요한 메모리를 할당한 다음 계산을 수행합니다. 따라서 하나의 메모리 할당만 수행됩니다