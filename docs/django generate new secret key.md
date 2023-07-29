---
aliases: 
tags: 
description:
title: django generate new secret key
created: 2023-07-29T17:47:04
updated: 2023-07-29T17:50:01
---
- <https://stackoverflow.com/a/57678930/21369350>

1.  `django-admin shell` 명령을 통해 인터랙티브 파이썬 셸을 연다.
2. 다음 코드를 친다.

```python
from django.core.management.utils import get_random_secret_key  
get_random_secret_key()
```
