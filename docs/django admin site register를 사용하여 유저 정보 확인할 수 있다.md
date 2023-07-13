---
description:
aliases: 
tags: 
created: 2023-06-05T14:23:21
updated: 2023-07-11T15:21:09
title: django admin site register를 사용하여 유저 정보 확인할 수 있다
---
- [purpose of admin.py {SOF}](https://stackoverflow.com/a/47753254)
- 
`admin.py`에 다음과 같은 코드를 작성하였더니, `localhost:8000/admin` 페이지에 새로운 애트리뷰트가 생겼다.

```python
"""admin/ route에서 보여줄 정보들에 대하여 정의한다."""
from django.contrib import admin
from django.contrib.auth.admin import UserAdmin
from .models import User


class Admin(UserAdmin):
    model = User
    list_display = ["email", "nickname", "is_staff"]


admin.site.register(User, Admin)
```

> The admin.py file is used to display your models in the Django admin panel. You can also customize your admin panel.

