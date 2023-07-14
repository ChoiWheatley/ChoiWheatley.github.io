---
description:
aliases: 
tags: 
created: 2023-05-17T14:52:16
updated: 2023-07-11T15:21:07
title: __getitem__ index and slice iteration in python
---
- [GFG](https://www.geeksforgeeks.org/__getitem__-in-python/)
- [`object.__getitem__` python documentation](https://docs.python.org/3/reference/datamodel.html#object.__getitem__)
커스텀 링크드 리스트를 만들다가 발생한 문제 브리핑.

# What is getitem

아래와 같이 마지막 원소를 제외한 나머지를 순회하면서 `ret`에 문자열을 끼워넣고 싶었는데 안된다고 한다. 자세히 들여다보니 `__getitem__` 던더함수가 없다고 나오네.
```python

class LinkedList:

    def __init__(self):
        init = Node(0, None)
        self.head = init
        self.tail = init
        self.count = 0

	# ... 중략

    def __str__(self):
        cur = self.head.next
        ret = "[ "
        for elem in self[0:-1]:
            ret += str(elem) + ", "

        ret += " ]"
        return ret
```

`__str__` 메서드 안에 `self[0:-1]` 구문에서 문제가 터지고 말았다.

두 브라켓 사이에 별에 별 걸 다 집어넣어본 경험이 있는 우리는... 이제 그 실체를 마주할 때가 되었다.

> Note that the special interpretation of negative indexes (if the class wishes to emulate a [sequence](https://docs.python.org/3/glossary.html#term-sequence) type) is up to the [`__getitem__()`](https://docs.python.org/3/reference/datamodel.html#object.__getitem__ "object.__getitem__") method

1. 음수 인덱스
2. 슬라이스 객체
3. 문자열 (매핑을 위하여)
4. 임의의 객체

# Implementing getitem

그래서, 임의의 타입을 일일이 `isinstance`를 찍어가면서 해야한단 거야? => No! [SOF - use exception handling!](https://stackoverflow.com/questions/22151335/implementing-getitem)

slice 타입을 어떻게 처리하지? [[slice.indices - python]]

빙챗한테 `__getitem__` 어떻게 구현하냐고 물어봤다. 굉장히 만족스러운 답변을 내놓았다. 

The `__getitem__` method receives a slice object when the object is sliced. You can look at the `start`, `stop`, and `step` members of the slice object to get the components for the slice¹.

Here's an example implementation of `__getitem__` that handles both indexing and slicing:

```python
class MyClass:
    def __init__(self, data):
        self.data = data

    def __getitem__(self, index):
		# 아래 보면 `isinstance`를 사용하는게 가독성에 더 좋을지도
        if isinstance(index, slice):
            start, stop, step = index.indices(len(self.data))
            return [self.data[i] for i in range(start, stop, step)]
        else:
            return self.data[index]
```

This implementation checks if the `index` parameter passed to `__getitem__` is a slice object. If it is, it uses the `indices` method of the slice object to get the start, stop and step values for the slice. It then uses a list comprehension to return a new list containing the sliced data. If `index` is not a slice object, it simply returns the element at that index in the data.

Source: Conversation with Bing, 5/17/2023
(1) python - Implementing slicing in __getitem__ - Stack Overflow. https://stackoverflow.com/questions/2936863/implementing-slicing-in-getitem.
(2) 3. Data model — Python 3.11.3 documentation. https://docs.python.org/3/reference/datamodel.html.
(3) Implementing slicing in __getitem__ - GeeksforGeeks. https://www.geeksforgeeks.org/implementing-slicing-in-__getitem__/.
(4) Fix Python – Implementing slicing in __getitem__. https://fixpython.com/2022/11/fix-python-implementing-slicing-in-getitem-po1tjgr/.
