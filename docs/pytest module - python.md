---
description:
aliases: 
tags: 
created: 2023-05-02T13:24:49
updated: 2023-07-15T21:33:03
title: pytest module - python
---

- 자동으로 `assert`를 수행해준다고 한다. 먼저 `pip install -U pytest`를 사용하여 스크립트를 다운받아야 한다.
- [conventions for python test discovery](https://docs.pytest.org/en/7.3.x/explanation/goodpractices.html#test-discovery) 에 따르면, `test_*.py`, `*_test.py`에 부합하는 파일을 찾고, 그 안에서 `test` prefixed test function 또는 `Test` prefixed test Class를 찾는다.
- 테스트 클래스에서 이때 중요한 점이 `__init__` 메서드가 없어야 한다.
