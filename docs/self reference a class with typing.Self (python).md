---
description:
aliases: 
tags: 
created: 2023-05-17T13:06:04
updated: 2023-07-15T21:33:03
title: self reference a class with typing.Self (python)
---
- [SOF question - annotate a method that returns Self](https://stackoverflow.com/questions/66526297/python-how-to-type-anotate-a-method-that-returns-self) 

해결방안: `typing.Self`를 사용하면 된다. 다만, python3.11이상 버전에서만 된다는거 알고가셈.

python version < 3.11인 경우에도 해결방안이 존재하긴 하다. `typing-extensions` 패키지를 설치하여 `from typing_extensions import Self`하면 된다.
