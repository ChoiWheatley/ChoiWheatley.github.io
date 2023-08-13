---
aliases: 
tags: 
description:
title: sliding window {python} {itertools.islice} {collections.deque}
created: 2023-08-13T16:14:51
updated: 2023-08-13T16:18:56
---
[doc {itertools-recipe}](https://docs.python.org/3/library/itertools.html#itertools-recipes)  
[sof](https://stackoverflow.com/a/6822773)

deque의 `maxlen` 키워드인자를 활용하여 append와 동시에 앞의 원소를 제거하면서(윈도우의 사이즈를 유지하면서) 슬라이딩 시킬 수 있다.

이때 `islice`를 사용한 이유는 아직 consume 되지 않은 이터레이터 또한 슬라이딩이 가능하게 만들기 위해서이다. 따라서 `yield` 키워드를 사용, 늦은 순회를 가능하게 만들었다.

itertools-recipes의 슬라이딩 윈도우 쪽을 참고해보자.

```python
def sliding_window(iterable, n):
    # sliding_window('ABCDEFG', 4) --> ABCD BCDE CDEF DEFG
    it = iter(iterable)
    window = collections.deque(islice(it, n), maxlen=n)
    if len(window) == n:
        yield tuple(window)
    for x in it:
        window.append(x)
        yield tuple(window)
```

만약 이터레이터 안쓰고 당장 리스트를 가지고 슬라이딩하고 싶다면 slicing과 two pointer 개념을 활용하면 된다.

```python
seq = [0, 1, 2, 3, 4, 5]
window_size = 3

for i in range(len(seq) - window_size + 1):
    print(seq[i: i + window_size])
```
