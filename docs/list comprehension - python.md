---
description:
aliases: 
tags: 
created: 2023-05-18T00:01:12
updated: 2024-11-23T11:55:03
title: list comprehension - python
---

# List Comprehension

`for` + `append` 보다 속도도 빠르고 가독성도 좋은데 안 쓸이유가 없잖아?

구문:

```python
[{{expr}} for {{variable}} in {{iterable}} {{condition}}]
```

```python
print([i for i in range(10)])
# if문을 넣어 필터링도 가능쓰
print([i for i in range(10) if i % 2 == 0])
list(range(0, 10, 2))  # 이것도 가능쓰
```

```python
# for 중첩도 가능하다. 아래는 cartesian product를 하는 구구단을 보여준다.
[[i, j, i * j] for i in range(2, 8) for j in range(2, 8)]
```
