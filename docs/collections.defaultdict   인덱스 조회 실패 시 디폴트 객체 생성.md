---
description:
aliases: 
tags: 
created: 2023-05-23T21:28:44
updated: 2023-07-15T21:30:21
title: collections.defaultdict   인덱스 조회 실패 시 디폴트 객체 생성
---
- https://docs.python.org/3/library/collections.html#collections.defaultdict
- [[파이썬 알고리즘 인터뷰 - 95가지 알고리즘 문제 풀이로 완성하는 코딩테스트]] p.293에서 커스텀 해시노드 (해시 체이닝 사용)를 자동으로 생성하기 위해 사용했다. 이 경우, 인덱싱에 실패하게 되면 `None`을 리턴하는 것이 아니라 그 자리에서 바로 디폴트 객체(아무 인자 없는 생성자)를 생성하여 리턴하게 된다.
- 버그찾는데 어려울 수도 있으므로, 충분한 실험을 한 뒤에 사용하자.

```python
s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]
d = defaultdict(list)
for k, v in s:
    d[k].append(v)

sorted(d.items())
[('blue', [2, 4]), ('red', [1]), ('yellow', [1, 3])]
```

When each key is encountered for the first time, it is not already in the mapping; so an entry is automatically created using the [`default_factory`](https://docs.python.org/3/library/collections.html#collections.defaultdict.default_factory "collections.defaultdict.default_factory") function which returns an empty [`list`](https://docs.python.org/3/library/stdtypes.html#list "list"). The `list.append()` operation then attaches the value to the new list. When keys are encountered again, the look-up proceeds normally (returning the list for that key) and the `list.append()` operation adds another value to the list. This technique is simpler and faster than an equivalent technique using [`dict.setdefault()`](https://docs.python.org/3/library/stdtypes.html#dict.setdefault "dict.setdefault"):
