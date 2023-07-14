---
description:
aliases: 
tags: 
created: 2023-05-18T00:04:26
updated: 2023-07-11T15:21:08
title: lambda - python
---

물론 이름을 부여할 수 있긴 함. 변수에 할당시키면 되거든.

일반 함수랑 다른점은 **참조 카운팅이 없다** 라는 것이다. => 사용된 다음 소멸된다...? 변수가 소거되는거에 대한 규칙은 다음에 알려준다.

lambda가 사용되는 곳
- map
- filter
- max
- min
- sorted

재사용이 필요해 보이는 건 함수를 선언하고, 일회성으로 끝날 것 같으면 람다를 사용하라.

```python
### 아래의 익명함수들은 calculator가 카운팅을 하게 된다. 따라서 여러번 재사용이 가능하다. 
calculator = [
    lambda x, y: x + y,
    lambda x, y: x - y,
    lambda x, y: x * y,
    lambda x, y: x / y,
]

for calc in calculator:
    print(f'calc result between 1, 2 is {calc(1, 2)}')
```

```python
def f():
    return lambda x, y: x + y

print(f()(1, 2))

# 람다를 클로저로 사용할 수도 있다.
def ff():
    return lambda x: lambda y: x + y

print(ff()(1)(2))
```
