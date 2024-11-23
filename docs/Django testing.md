---
aliases: 
tags: 
description:
title: Django testing
created: 2024-11-23T21:00:09
updated: 2024-11-23T21:06:46
---
- ref: 

## `unittest.TestCase` 와 `django.test.TestCase`의 차이점

If your tests rely on database access such as creating or querying models, be sure to create your test classes as subclasses of [`django.test.TestCase`](https://docs.djangoproject.com/en/5.1/topics/testing/tools/#django.test.TestCase "django.test.TestCase") rather than [`unittest.TestCase`](https://docs.python.org/3/library/unittest.html#unittest.TestCase "(in Python v3.13)").

Using [`unittest.TestCase`](https://docs.python.org/3/library/unittest.html#unittest.TestCase "(in Python v3.13)") avoids the cost of running each test in a transaction and flushing the database, but if your tests interact with the database their behavior will vary based on the order that the test runner executes them. This can lead to unit tests that pass when run in isolation but fail when run in a suite.

- `django.test.TestCase`: 테스트를 실행할때 테스트용 `default` 데이터베이스를 생성하여 거기에서 테스트를 진행한다. 테스트가 끝나면 `default` 데이터베이스를 삭제한다.
- `unittest.TestCase`: 테스트를 실행할때 데이터베이스 신경을 쓰지 않는다. 
