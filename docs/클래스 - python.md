---
description:
aliases: 
tags: 
created: 2023-05-01T13:05:45
updated: 2023-07-11T15:20:17
title: 클래스 - python
---
- `class MyClass(object):` 로 시작한다. 저 괄호 안에 들어가는 건 뭐지? => `object` 타입을 상속한다는 의미임.
- 메서드: [[0013 Rust 🦀|rust]]처럼 파라메터의 첫번째 녀석으로 `self`가 들어간다.
- 클래스 변수 : C/C++/Java의 static class member
- `__init__` : 생성자임. 다른 메서드와 마찬가지고 첫번째 인자로 `self`가 들어간다는거
- [dunder methods](https://docs.python.org/3/reference/datamodel.html#index-34) : 예약된 파이썬 기본내장 특별 메서드들임. 더블 언더바 `__` 사이에 메서드 이름이 끼어있다.
	- [`object.__getattr__(self, name)`](https://docs.python.org/3/reference/datamodel.html#object.__getattr__) 이름이 직관적이진 않지만 클래스에 해당 속성이 없어 `AttributeError` 에러가 발생할 시 호출되는 코드이다. 이 함수를 내가 별도로 구현하여 오버라이드(?) 할 수 있다.
- 