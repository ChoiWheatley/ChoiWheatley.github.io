---
aliases: 
tags: 
description:
title: check every element from sequence is same {python} {functools.reduce}
created: 2023-08-13T16:36:38
updated: 2023-08-13T16:37:58
---

```python
from functools import reduce

seq = [1,1,1,1,1,1,1,1,1,1]
reduce(lambda x, y: x == y, seq)
```
