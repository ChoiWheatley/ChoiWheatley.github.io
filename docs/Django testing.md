---
aliases: 
tags: 
description:
title: Django testing
created: 2024-11-23T21:00:09
updated: 2024-12-05T00:36:50
---
- ref: <https://docs.djangoproject.com/en/5.1/topics/testing/>
	- [Writing and running tests](https://docs.djangoproject.com/en/5.1/topics/testing/overview/)  | Django의 테스트 프레임워크는 Python의 unittest 모듈을 활용하여 개발자가 애플리케이션 테스트를 작성하고 실행해 코드의 신뢰성과 정확성을 보장할 수 있게 합니다.
	- [Testing tools](https://docs.djangoproject.com/en/5.1/topics/testing/tools/) | Django는 테스트 클라이언트와 RequestFactory를 제공하여 개발자가 애플리케이션의 뷰와 요청을 프로그램적으로 테스트할 수 있도록 지원합니다.
	- [Advanced testing topics](https://docs.djangoproject.com/en/5.1/topics/testing/advanced/) | Django는 RequestFactory를 통해 뷰를 독립적으로 테스트하고, 다중 데이터베이스 설정 및 클래스 기반 뷰의 테스트를 지원합니다.

## `unittest.TestCase` 와 `django.test.TestCase`의 차이점

If your tests rely on database access such as creating or querying models, be sure to create your test classes as subclasses of [`django.test.TestCase`](https://docs.djangoproject.com/en/5.1/topics/testing/tools/#django.test.TestCase "django.test.TestCase") rather than [`unittest.TestCase`](https://docs.python.org/3/library/unittest.html#unittest.TestCase "(in Python v3.13)").

Using [`unittest.TestCase`](https://docs.python.org/3/library/unittest.html#unittest.TestCase "(in Python v3.13)") avoids the cost of running each test in a transaction and flushing the database, but if your tests interact with the database their behavior will vary based on the order that the test runner executes them. This can lead to unit tests that pass when run in isolation but fail when run in a suite.

- `django.test.TestCase`: 테스트를 실행할때 테스트용 `default` 데이터베이스를 생성하여 거기에서 테스트를 진행한다. 테스트가 끝나면 `default` 데이터베이스를 삭제한다.
- `unittest.TestCase`: 테스트를 실행할때 데이터베이스 신경을 쓰지 않는다. 

