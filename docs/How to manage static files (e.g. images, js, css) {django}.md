---
aliases: 
tags: 
description:
created: 2023-06-20T23:36:29
updated: 2023-07-18T23:09:17
title: How to manage static files (e.g. images, js, css) {django}
---


- [doc.com](https://docs.djangoproject.com/en/4.2/howto/static-files/)
- [관련 bookstore 리포지토리 PR 내용](https://github.com/ESTsoft-Book-Project/bookstore/pull/50)
- [learndjango.com](https://learndjango.com/tutorials/django-static-files-and-templates)
- [[aws s3 static files in django]]
---
- settings.py
    - `STATICFILES_DIRS`에 `static` 폴더 추가하여 정적파일(getCookie.js) 관리
- base.html: `{% load static %}` & `{% static 'js/getCookie.js %}`를 사용하여 정적파일을 링크 (이렇게 안하면 라우트 뒤에 src가 붙습니다 ⚠️)

> [!note] Your project will probably also have static assets that aren’t tied to a particular app. In addition to using a `static/` directory inside your apps, you can define a list of directories ([`STATICFILES_DIRS`](https://docs.djangoproject.com/en/4.2/ref/settings/#std-setting-STATICFILES_DIRS)) in your settings file where Django will also look for static files. For example:

```python
STATICFILES_DIRS = [
    BASE_DIR / "static",
    "/var/www/static/",
]
```

# `static()`

[docs.com](https://docs.djangoproject.com/en/3.2/ref/urls/#static)

> Helper function to return a URL pattern for serving files in **debug** mode:

이상한 코드를 보았다. <https://github.com/ESTsoft-Book-Project/bookstore/blob/25379d55f885264568e12dd9d196658858a4fd86/core/urls.py#L23-L29>

`urlpatterns` 뒤에 `static()`을 `+` 연산으로 이어붙인 것 같은데, 이건 도대체 뭐냐?

```python
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    # ... the rest of your URLconf goes here ...
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```
