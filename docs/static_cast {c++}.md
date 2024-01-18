---
aliases: 
tags: 
description:
title: static_cast {c++}
created: 2024-01-10T00:43:28
updated: 2024-01-18T22:28:01
---
- [[C++]]
- <https://en.cppreference.com/w/cpp/language/static_cast>
---

## C-style 캐스팅과 `std::static_cast`의 차이점이 무엇인가요?

> `static_cast<target-type> (expression)`, with extensions: pointer or reference to a derived class is additionally allowed to be cast to pointer or reference to unambiguous base class (and vice versa) even if the base class is inaccessible (that is, this cast ignores the private inheritance specifier). Same applies to casting pointer to member to pointer to member of unambiguous non-virtual base;

상속관계도에서 **down cast**를 수행한다. 따라서 런타임에 별도의 타입체크를 하지 않으며, 컴파일 타임에 안전하게 Base 타입 객체를 Derived 타입으로 변환할 수 있다. 물론 캐스팅 하려는 객체가 Derived 타입 인스턴스여야만 하다.
