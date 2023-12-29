---
aliases: 
tags: 
description:
title: collections.Counter, most_common 아이템 개수 계산 {python}
created: 2023-12-23T14:27:41
updated: 2023-12-23T14:29:47
---
- [[0014 Python 🐍]]
___

원소들을 자동으로 계산해 주기 때문에 매우 편리함

```python
a = [1, 2, 3, 4, 5, 5, 5, 6, 6]
b = collections.Counter(a)
b
# Counter({5:3, 6:2, 1:1, 2:1, 3:1, 4:1})
```

아래의 `most_common` 메서드는 인자로 들어온 개수만큼 카운트가 높은 놈들을 리턴한다.

```python
b.most_common(2)
# [(5, 3), (6, 2)]
```
