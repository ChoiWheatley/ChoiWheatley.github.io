---
aliases: 
tags: 
description:
created: 2023-06-29T10:26:57
updated: 2023-07-15T21:33:04
title: getattr, setattr {python}
---
- [docs.org](https://docs.python.org/3/library/functions.html#setattr)
- `dict`가 아닌 일반 객체는 `["attr"]` 용법을 사용하기 위해선 반드시 `__getitem__`,`__setitem__` 매직 메서드를 구현해야 한다. 만약 내가 만든 객체가 아니라면?객체의 필드에 접근해야 하는데 하필이면 필드가 이러저러한 이유로 문자열로 들어온다면?
- 줄곧 생각한 방식은 mapper를 만들어 일일이 호출을 하는 로직을 만드는 것이었다. 엄청난 양의 보일러플레이트 코드가 생기겠지
- 이 문서가 나온 이유가 바로 저 내장함수 덕분이다. `getattr(x, 'foobar')`는 `x.foobar`와 동일하다. 마찬가지로 `setattr(x, 'foobar', 123)`은 `x.foobar = 123`과 동일하다.
