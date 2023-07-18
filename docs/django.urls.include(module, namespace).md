---
description:
aliases: 
tags: 
created: 2023-06-01T15:21:01
updated: 2023-07-19T01:05:57
title: django.urls.include(module, namespace)
---

- [django.urls.include {doc}](https://docs.djangoproject.com/en/4.2/ref/urls/#django.urls.include)
- 모듈이름을 인자로 받는다. 타 모듈을 임포트 할 필요 없이 `urls.py`를 가지고 있는 장고앱을 명시만 하면 알아서 연결해주는듯.

[[0014.1 Django 🎈]]예제를 가져와보자.

```python
# blogtutorial/ursl.py ==> Root application
urlpatterns = [
    path("admin/", admin.site.urls),
    path("", include("blog.urls")),
    path("accounts/", include("users.urls")),
]
```

`blog/urls.py` ==> for base templates and articles

```python

urlpatterns = [
    # root path => blog/templates/blog/index.html
    # {% url 'index' %} will lead user index.html
    path("", Index.as_view(), name="index"),
    path("tinymce/", include("tinymce.urls")),
    path("<int:pk>/", DetailArticleView.as_view(), name="detail_article"),
    path("<int:pk>/like", LikeArticle.as_view(), name="like_article"),
]
```

`users/urls.py` ==> pages for login/out/register

```python
urlpatterns = [
    path(
        "login/",
        auth_views.LoginView.as_view(template_name="users/login.html"),
        name="login",
    ),
    path(
        "logout/",
        auth_views.LogoutView.as_view(template_name="users/logout.html"),
        name="logout",
    ),
    path("register/", RegisterView.as_view(), name="register"),
]
```

`env/lib/python3.11/site-packages/tinymce/urls.py ` ==> pip로 설치했던 tinymce

```python
urlpatterns = [
    path("spellchecker/", views.spell_check, name="tinymce-spellcheck"),
    path("flatpages_link_list/", views.flatpages_link_list, name="tinymce-linklist"),
    path("compressor/", views.compressor, name="tinymce-compressor"),
    path("filebrowser/", views.filebrowser, name="tinymce-filebrowser"),
]
```

`nav.html` ==> django template으로 `{% url %}` 태그를 사용하는 구문. 템플릿 url은 reverse 문법을 따른다는 것 잊지말자! [[django reverse]]

```html
<div class="header">
    <h1><a href="{% url 'index' %}">title</a></h1>
    <div class="header-click">
        <a href="{% url 'users:signin' %}">로그인</a>
        <a href="{% url 'users:signup' %}">회원가입</a>
        <button type="button">로그아웃</button>
    </div>
</div>
```
