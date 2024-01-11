---
aliases: 
tags: 
description:
title: 커스텀 타입을 string으로 변환하는 방법 {C++}
created: 2024-01-11T17:01:44
updated: 2024-01-11T17:01:49
---
- [[C++]]
- [toString function or (std::string) cast overload in c++](https://stackoverflow.com/a/41200166)
---

- `.str()` notation을 활용하여 string, stringstream, match_result 변환처럼 적용하기
- `to_string()` 자유함수를 구현하여 숫자⇒문자열 변환처럼 적용하기
- `std::string()` 방식으로 명시적 캐스팅을 수행하기
    - `explicit operator std::string() const`
- `friend std::ostream& operator<<()` 를 구현하여 stream으로 작성할 수도 있고.
