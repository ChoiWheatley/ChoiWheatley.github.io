---
description:
aliases: 
tags: 
created: 2023-05-02T13:03:34
updated: 2023-07-15T21:33:03
title: python package
---
- https://docs.python.org/3/reference/import.html#regular-packages
- 파이썬 환경 내에서 모든 것은 모듈이다. 이 중에서 `__init__.py` 파일을 가지고 있는 폴더를 패키지라고 부르는데, 이 패키지도 모듈의 일종이다. 
- 어떤 패키지의 모듈을 import 하게 되면 암묵적으로 `__init__.py` 스크립트가 실행된다. 물론 내용물이 없어도 상관은 없다.

	```
	parent/
    __init__.py
    one/
        __init__.py
    two/
        __init__.py
    three/
        __init__.py
	```
