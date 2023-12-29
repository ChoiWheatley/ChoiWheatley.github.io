---
aliases: 
tags: 
description:
title: collections.OrderedDict 순서를 보장하는 딕셔너리 {python}
created: 2023-12-23T14:30:10
updated: 2023-12-23T14:33:45
---
- [[0014 Python 🐍]]
---
3.7버전 이후부터는 인덱스를 사용하여 입력 순서를 보장하도록 수정됐지만 코테에서 어떤 버전의 인터프리터를 사용하는지 알 수가 없기 때문에 무턱대고 기본 dict를 사용하는 것은 위험하다. 따라서 하위호환성을 위해 남겨진 `collections.OrderedDict`를 사용하는 것이 중요하다.

```python
collections.OrderedDict({'a': 3, 'b': 4, 'c': 1, 'd': 2})
```
