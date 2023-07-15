---
description:
aliases: 
tags: 
created: 2023-05-17T17:31:36
updated: 2023-07-15T21:33:03
title: type hint cheatsheet
---
<iframe src="https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html" allow="fullscreen" allowfullscreen="" style="height:100%;width:100%; aspect-ratio: 16 / 9; "></iframe>

[[generator function - python]]

```python
# A generator function that yields ints is secretly just a function that
# returns an iterator of ints, so that's how we annotate it
def gen(n: int) -> Iterator[int]:
    i = 0
    while i < n:
        yield i
        i += 1
```
