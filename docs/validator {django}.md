---
aliases: 
tags: 
description:
created: 2023-06-27T10:57:27
updated: 2023-07-15T21:33:03
title: validator {django}
---
- [docs.com](https://docs.djangoproject.com/en/4.2/ref/forms/validation/)

Form의 유효성 검사를 진행하는 것이 바로 Validator이다. 기본 내장 유효검사인 EmailValidator, SlugValidator 등이 있는데, bookstore에서 Product의 stock을 넘어선 Cart의 quantity가 담기지 않게 유효성 검사를 진행할 것이다.

아니다. 그냥 코드 중간에 끼워넣었다. validator는 필드에 하나에 대한 인자만 가지고 유효성 검사를 수행하는데, 나는 stock과 quantity를 모두 비교해야 하기 때문에 올바르지 않다.
