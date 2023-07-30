---
aliases: 
tags: 
description:
title: ViewSets {drf}
created: 2023-07-30T22:49:59
updated: 2023-07-31T04:12:07
---

# INDEX

- [doc](https://www.django-rest-framework.org/api-guide/viewsets/#viewsets)
- [[drf 에서 redirect url에 대한 정보를 ViewSet에 담는법 {drf}]]

# urlconf를 자동으로 생성해주는 `View`들의 집합

`Router` 객체를 통하여 `urlpatterns`를 자동으로 생성할 수 있다. 그러기 위해선 `ViewSet`을 구현하여야 하며, `list`, `retrieve` 메서드를 구현하여야 한다. 이름에서부터 알 수 있듯이, 두 메서드는 각각 모든 데이터 참조와 구체적인 데이터 하나 참조를 의미한다.

`core/urls.py`

```python
from django.contrib import admin
from django.urls import path
from rest_framework.routers import DefaultRouter
from users.views import MemberViewSet

router = DefaultRouter()
router.register(r"users", MemberViewSet, basename="user")

urlpatterns = [
    path("admin/", admin.site.urls),
] + router.urls
```
