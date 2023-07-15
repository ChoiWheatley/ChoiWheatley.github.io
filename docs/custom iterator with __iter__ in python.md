---
description:
aliases: 
tags: 
created: 2023-05-10T17:40:57
updated: 2023-07-15T21:30:21
title: custom iterator with __iter__ in python
---
- https://stackoverflow.com/a/20214464
- [python __iter__ and __next__ converting an object into an iterator](https://www.geeksforgeeks.org/python-__iter__-__next__-converting-object-iterator/)
- [SOF - How to build a basic iterator?](https://stackoverflow.com/questions/19151/how-to-build-a-basic-iterator)
- https://realpython.com/introduction-to-python-generators/

원칙:
1. `__iter__`를 구현하여야 한다. 이 매직 메서드 안에 순회를 위한 인덱스라던가 이것저것을 초기화 하는 코드를 넣으면 좋습니다. 
2. `__iter__`는 반드시 `__next__` 매직 메서드를 구현한 객체를 리턴하여야 한다.
3. `__next__` 메서드는 컨테이너의 끝에 도달하는 등 순회가 끝났을 때 반드시 `StopIteration` 에러를 일으켜야만 한다.

예제로 `range` 비스무리한 커스텀 이터레이터를 만들어보자.

```python
class MyIterator:

    def __init__(self, stop):
        self.currentValue = 0
        self.stop = stop

    def __iter__(self):
        return self

    def __next__(self):
        if self.currentValue >= self.stop:
            raise StopIteration
        result = self.currentValue
        self.currentValue += 1
        return result

my_iterator = MyIterator(5)

for i in my_iterator:
    print(i)
```

under the hood...
1. `for`가 `__iter__`를 호출한다. 내부적으로 초기화 작업을 수행한다.
2. 순회를 하면서 `__next__`를 호출한다. 이때 이터레이터는 내부 상태를 바꾸는 작업을 수행한다.

### `iter`, `next` builtin functions

그냥 `iter` 는 `__iter__`를 실행시켜 이터레이터 오브젝트를 반환받고, `next`는 이터레이터의 상태를 변화시고 결과값을 받아낸다.

```python
my_iterator = MyIterator(5)
it = iter(my_iterator)
print(it)
print(next(it))  # 0
print(next(it))  # 1
print(next(it))  # 2
print(next(it))  # 3
print(next(it))  # 4
print(next(it))  # raise StopIteration
```

### Unpacking Iterators

순회를 굳이 하지 않고, 몇개의 원소들을 임의로 분해(혹은 언팩)할 수 있다. 이건 좀 좋은데?

```python
a, b, c, d, e = MyIterator(5)
print(a, b, c, d, e)
```

### Generator which generates iterator-like control block

[[generator function - python]]으로 옮겨감
