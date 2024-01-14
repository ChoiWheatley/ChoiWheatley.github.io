---
aliases: 
tags: 
description:
title: static_cast {c++}
created: 2024-01-10T00:43:28
updated: 2024-01-14T17:26:44
---
- [[C++]]
- <https://en.cppreference.com/w/cpp/language/static_cast>
---

## C-style 캐스팅과 `std::static_cast`의 차이점이 무엇인가요?

> `static_cast<target-type> (expression)`, with extensions: pointer or reference to a derived class is additionally allowed to be cast to pointer or reference to unambiguous base class (and vice versa) even if the base class is inaccessible (that is, this cast ignores the private inheritance specifier). Same applies to casting pointer to member to pointer to member of unambiguous non-virtual base;
