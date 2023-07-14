---
description:
aliases: 
tags: 
created: 2023-05-08T17:03:16
updated: 2023-07-11T15:20:17
title: 파이썬 함수를 호출할 때 인자 순서를 바꿔써도 된다 + default value
---
```python
def f(a, b, c):
    print(a, b, c)

# f() # error
# f(100, 10) # error
f(a=100, b=200, c=300)
f(c=300, a=100, b=200) #실무에서 이렇게 순서를 바꿔서 넣지 않고 보통 순서를 지켜줍니다.

# 실무에서 언제 사용이 될까요?
# 파라미터, 아규먼츠가 매우 많을 때
# 이런 함수가 나오면 이 함수가 무엇을 뜻하는 것일까요?
# addNewControl("Title", 20, 50, 100, 50, 200, 300, 150)
# addNewControl(title="Title", height=20, width=50, textlen = 100, ...)
# 어? 컨트롤 박스를 만드는 구나...
```

아규먼트로 구체적인 이름과 ` = `를 넣어주면 순서에 상관없이 넣을 수 있다...! 
CPP의 default value 규칙과 같이 마지막 인자부터 default value를 넣어야 하는 것으로 보인다.

```python
def f(a, b=20, c=10): # 순서대로 안주어야 하기 때문에 a에 값을 넣지 않았습니다.
    print(a, b, c)

# f() # error
f(100)
f(100, 10)
f(a=100, b=200, c=300)
f(c=300, a=100, b=200
```