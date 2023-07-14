---
description:
aliases: 
tags: 
created: 2023-05-18T00:17:36
updated: 2023-07-11T15:21:08
title: module and package (python)
---
# Module and Package

현재 폴더에 `test1.py`라는 파일을 생성하였고 `hello` 함수, `Human` 클래스, `name`, `age` 변수를 이쪽에서 임포트 할 수 있다.

```python
import test1

test1.name
```

`import`는 모듈이름이나 폴더나 다 들어갈 수 있다

```python
`import`는 모듈이름이나 폴더나 다 들어갈 수 있다

from one.two import three

print(three.name)
```

`as` 를 사용하여 약칭을 사용할 수도 있다.

```python
`as` 를 사용하여 약칭을 사용할 수도 있다.

import test2 as test1
import pandas as p
import numpy as n

print(test1.name)
```