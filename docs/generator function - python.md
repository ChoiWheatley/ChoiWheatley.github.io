---
description:
aliases: 
tags: 
created: 2023-05-17T17:37:42
updated: 2023-07-11T15:21:08
title: generator function - python
---
[[2023-05-11 estsoft - python - inheritance, linked-list, method-overriding, MRO, private-member, iterator, generator, module, file-io, excel]]
- https://realpython.com/introduction-to-python-generators/

# Generator function generates iterator-like control block

`yield` 문법이 나온다. `yield` 키워드를 가지고 있는 함수를 generator function이라고 부른다. 함수를 suspend 하고 `generator` 객체를 리턴하는 역할을 가지고 있다. 따라서 함수의 상태가 어딘가 저장이 되고 재호출이 되기를 기다린다. 해당 함수가 외부에서 다시 호출이 되면 `yield` 이후 문장들을 다시 실행하기 시작한다. 

`yield`를 사용하여 리턴한 `yield` 객체로부터 데이터 뽑아내려면 `next` 내장 함수를 사용하자. 완전 이터레이터와 사용방법이 동일하지? 
```python
def my_yield():
    print('<my_yield> before yield')
    yield 1
    print('<my_yield> after yielding 1')
    yield 2
    print('<my_yield> after yielding 2')
    yield 3
    print('<my_yield> after yielding 3')

generated = my_yield()
print(next(generated))  # 1
print(next(generated))  # 2
print(next(generated))  # 3
print(next(generated))  # raise StopIteration
```

`my_yield` 함수에 대한 호출은 마치 리스트로부터 `iter`를 호출한 것 마냥 새로운 제너레이터 객체를 리턴하는 것 같다. 그래서 멍청하게 `my_yield`를 여러 번 호출했다가 잠깐 멘붕에 빠졌다.
```python
print(next(my_yield()))  # 1 
print(next(my_yield()))  # 1 
print(next(my_yield()))  # 1 
print(next(my_yield()))  # 1 
print(next(my_yield()))  # 1 
print(next(my_yield()))  # 1 
...
```

나중에 멀티스레딩과 네트워크 프로그래밍과 함께 하면 복잡하게 얽혀 고오급 프로그래머가 될 수 있다.

아래 코드는 무한루프에 걸린다.
```python
def gen():
	count = 0
	while True:
		yield count
		count += 1

for i in gen():
	print(i)
```

아래 코드는 무한한 짝수 정수를 게으르게 다루는 법에 대한 스니펫이다. `gen()`은 분명히 무한하지만 `zip`과 함께 사용하여 바운딩이 된다!
```python
def gen():
    count = 0
    while True:
        yield count
        count += 2


l = [10, 20, 30, 40, 50]
print(list(zip(l, gen())))
```

[[type hint cheatsheet]]에 따르면, 제너레이터 함수의 리턴값은 그냥 `Iterator`라고 한다.

```python
# A generator function that yields ints is secretly just a function that
# returns an iterator of ints, so that's how we annotate it
def gen(n: int) -> Iterator[int]:
    i = 0
    while i < n:
        yield i
        i += 1
```

- [ ] TODO: read
- [Generator Expressions](https://realpython.com/introduction-to-python-generators/#building-generators-with-generator-expressions)부터 읽기 시작하자.