---
aliases: 
tags: 
description:
title: collections.Counter, most_common 아이템 개수 계산 {python}
created: 2023-12-23T14:27:41
updated: 2023-12-29T22:45:21
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

## 예제 문제

<https://leetcode.com/problems/most-common-word/>

banned에 들어있지 않은 단어 중 가장 많이 카운트된 단어를 고르는 문제. 필터링 후 카운트 하는 방법을 사용하면 된다.

```python
class Solution4:
    """
    banned가 list였기 때문에 필터링에서 오랜 시간이 걸렸던 것이다. 따라서 이를 set으로 바꿔줬더니 90ms -> 38ms로 바뀌었다.
    """

    def mostCommonWord(self, paragraph: str, banned: List[str]) -> str:
        banned_set = set(banned)
        normalized = "".join(c.lower() if c.isalnum() else " " for c in paragraph)
        words = normalized.split()
        filtered = filter(lambda w: w and w not in banned_set, words)
        counter = Counter(filtered)
        return counter.most_common()[0][0]
```
