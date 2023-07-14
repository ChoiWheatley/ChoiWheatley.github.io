---
description:
aliases: 
tags: 
created: 2023-05-11T11:00:29
updated: 2023-07-11T15:21:10
title: 2023-05-11 estsoft - python - inheritance, linked-list, method-overriding, MRO, private-member, iterator, generator, module, file-io, excel
---
어디까지 했더라

# Class 이어하기

## Inheritance re

아쉽게도, 상속을 하는 *이유*에 대하여 설명이 조금 부족했던 것 같기도 하다. 파이썬에서의 상속도 Java, C++등 다른 OOP 언어들의 패러다임을 그대로 가지고 있겠지? 잠만... 파이썬에는 인터페이스가 없나?

![[파이썬에는 인터페이스가 없다]]

상속은 전에 확인했을 때 부모 클래스의 클래스 attr를 상속받는다고 했다. 하지만 인스턴스 attr은 상속받지 않는다. 다만, 애초에 파이썬의 인스턴스 attr의 개념이 다른 언어랑은 확연하게 다른지라... 그냥 코에 걸면 코걸이 식으로 된다고 이해하자.

## Linked List

이거 공부할 때만 해도 이터레이터를 배우지 않아서 걍 `next` 체이닝 했다. 이번엔 `LinkedList` 객체와 그 이터레이터 만들 수 있을지도!

==TODO== : [[Linked List in python]]

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None
    
    def link_to(self, next):
        self.next = next


노드1 = Node(10)
노드2 = Node(20)
노드3 = Node(30)

노드1.link_to(노드2)
노드2.link_to(노드3)
노드3.link_to(노드1)


print(노드1.data)
print(노드1.next.data)
print(노드1.next.next.data)
print(노드1.next.next.next.data)
```

## Method Overriding

정의: 부모에게서 상속받은 attr를 같은 이름으로 재선언하여 사용하는 것.

## Multiple Inheritance and `mro` builtin function

[[Multiple Inheritance and mro builtin function]]

## Private Attributes

[[private attributes (python)]]

# Iterator

[[custom iterator with __iter__ in python]]

# Decorator

[[decorator - python]]으로 옮겨감

# 모듈

[[module and package (python)]]

# 파일 입출력

[[file-io (python)]]
