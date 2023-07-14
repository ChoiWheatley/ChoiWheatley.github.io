---
aliases: 
tags: 
description:
created: 2023-06-19T15:16:49
updated: 2023-07-11T15:21:07
title: User authentication 관련 에러 (인증, 인가){django}
---
`NotImplementedError: Django doesn't provide a DB representation for AnonymousUser.` | [#16](https://github.com/ESTsoft-Book-Project/bookstore/issues/16) 

위의 대화에 따르면 유저의 인가와 관련한 몇가지 처리방법에 대한 방법을 알 수 있다.

## `login_required` decorator

로그인 여부를 살펴보고 예상치 못한 페이지 접근을 차단할 수 있으며, 필요한 경우 별도의 로그인 페이지로 리다이렉트도 할 수 있다.

## `is_authenticated`, `is_anonymous` field

기본적으로 장고의 `AbstractBaseUser`를 상속받은 모든 객체는 해당 필드를 가지고 있다. Django 인증 기능을 사용해 로그인한 사용자인지 여부에 따라 필드의 값이 변한다.