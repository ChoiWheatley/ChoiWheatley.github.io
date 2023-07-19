---
description: admin 페이지에 새 애트리뷰트 추가하기
aliases: 
tags: 
created: 2023-06-05T14:23:21
updated: 2023-07-19T14:46:03
title: add model in admin site with `admin.site.register` {django} {admin.py}
---
- [admin site {doc}](https://docs.djangoproject.com/en/4.2/ref/contrib/admin/)
- [Custom users and `django.contrib.admin` {doc}](https://docs.djangoproject.com/en/4.2/topics/auth/customizing/#custom-users-and-django-contrib-admin)
- [purpose of admin.py {SOF}](https://stackoverflow.com/a/47753254)  
- [bookstore/users/admin.py {GH}](https://github.com/ESTsoft-Book-Project/bookstore/blob/main/users/admin.py)  
`<app>/admin.py`에 다음과 같은 코드를 작성하였더니, `localhost:8000/admin` 페이지에 새로운 애트리뷰트가 생겼다.

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

## `AbstractBaseUser`를 상속한 커스텀 유저일 경우

Admin site에 모습을 드러내게 하기 위해서 반드시 `django.contrib.auth.admin.UserAdmin`을 확장하여야 한다. 자세한 사항은 [문서](https://docs.djangoproject.com/en/4.2/topics/auth/customizing/#custom-users-and-django-contrib-admin)와 아래 Note를 읽어보기 바란다.

> [!note]  
> If you are using a custom `ModelAdmin` which is a subclass of `django.contrib.auth.admin.UserAdmin`, then you need to add your custom fields to `fieldsets` (for fields to be used in editing users) and to `add_fieldsets` (for fields to be used when creating a user). For example:
>
> ```python
> from django.contrib.auth.admin import UserAdmin
> class CustomUserAdmin(UserAdmin):
>     ...
>     fieldsets = UserAdmin.fieldsets + ((None, {"fields": ["custom_field"]}),)
>     add_fieldsets = UserAdmin.add_fieldsets + ((None, {"fields": ["custom_field"]}),)
> ```
