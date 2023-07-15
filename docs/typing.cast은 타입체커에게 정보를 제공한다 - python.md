---
description:
aliases: 
tags: 
created: 2023-05-17T17:01:50
updated: 2023-07-15T21:33:03
title: typing.cast은 타입체커에게 정보를 제공한다 - python
---
[[type hint cheatsheet]]

<iframe src="https://docs.python.org/3/library/typing.html" allow="fullscreen" allowfullscreen="" style="height:100%;width:100%; aspect-ratio: 16 / 9; "></iframe>

cast는 타입체크를 전혀 하지 않는구나...

```python
some_none_value = None
node = cast(Node, some_none_value)
node.data # Runtime error
```
