---
description:
aliases: 
tags: 
created: 2023-05-09T16:37:46
updated: 2023-07-15T21:30:22
title: 20230509 estsoft - python - list comprehension, try except else, builtin functions, args and kwargs, lambda
---
{% raw %}

- [최승현의 구글 콜랩 링크](https://colab.research.google.com/drive/1gxoD01mjta80MkTOlrei1BHSUI0_k9-R#scrollTo=V6GhAqHXUW0h&line=9&uniqifier=1)

# List Comprehension

`for` + `append` 보다 속도도 빠르고 가독성도 좋은데 안 쓸이유가 없잖아?

구문:

```python
[{{expr}} for {{variable}} in {{iterable}} {{condition}}]
```

- condition쪽은 옵션이다. 필터링 할 때 아주 유용함.
- cartesian product를 얼마나 할 지는 잘 모르겠으나, `for` 중첩이 가능하기 때문에 일종의 구구단 생성기도 만들 수 있다.

	```python
	[[i, j, i * j] for i in range(2, 8) for j in range(2, 8)]
	```

# try - except - else

[[try - except - else - finally (python)]]

# 가짜 JSON 데이터를 사용한 연습문제 

[[가짜 JSON 데이터 생성기(json-generator)를 사용한 python 연습문제]]

# Builtin Functions는 니가 알아서 찾아봐라

# args, kwargs 가변인자와 가변 키워드 인자

[[args, kwargs 가변인자와 가변 키워드 인자]]

# lambda 익명함수

[[lambda - python]]

{% endraw %}
