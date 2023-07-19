---
description: admin 페이지에 새 애트리뷰트 추가하기
aliases: 
tags: 
created: 2023-06-05T14:23:21
updated: 2023-07-19T14:15:03
title: add model in admin page with `admin.site.register` {django}
---
- [purpose of admin.py {SOF}](https://stackoverflow.com/a/47753254)  
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
