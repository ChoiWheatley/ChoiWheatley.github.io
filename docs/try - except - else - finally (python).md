---
description:
aliases: 
tags: 
created: 2023-05-18T00:02:32
updated: 2023-07-15T21:33:03
title: try - except - else - finally (python)
---

{% raw %}

# Exception Handling

```python
try:
    # 예외가 발생할 가능성이 있는 코드
except {{option: specific error type}}:
    # 예외 발생 시 처리할 코드
else:
    # 예외 미 발생 시 처리할 코드
finally:
    # 예외고 뭐고 무조건 실행할 코드
    # 얘가 존재하는 이유는 예외 발생시에도 무조건 실행하기 때문에
    # 동기화 문제를 해결할 타이밍을 제공할 수 있다.
```

```python
try:
    i = 1
    j = 0
    x = i / j
except:
    print("divide by zero error")
else:
    print(x)
```

# Custom Error

간단하다. `Exception` 클래스를 상속하는 클래스를 구현하면 된다. 그리고 `__init__`에서 부모 클래스의 attr를 참조하는 `super` 얘가 뭐지 쨌든 를 사용하여 `super().__init__("Message")` 이런 식으로 메시지를 추가할 수 있다.

[super](https://www.geeksforgeeks.org/python-super/): Return a proxy object which represents the parent's class

```python
class FxxxedUpError(Exception):
    def __init__(self):
        super().__init__("Hey, you are gonna be fired soon 💀💀💀")


raise FxxxedUpError
```

{% endraw %}
