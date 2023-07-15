---
description:
aliases: 
tags: 
created: 2023-05-31T15:21:25
updated: 2023-07-15T21:30:20
title: django path(route, view, name)
---
- https://docs.djangoproject.com/en/4.2/ref/urls/
- [How Django processes a request {doc}](https://docs.djangoproject.com/en/4.2/topics/http/urls/#how-django-processes-a-request)

```python
urlpatterns = [
    path("index/", views.index, name="main-view"),
    path("bio/<username>/", views.bio, name="bio"),
    path("articles/<slug:title>/", views.article, name="article-detail"),
    path("articles/<slug:title>/<int:section>/", views.section, name="article-section"),
    path("blog/", include("blog.urls")),
    ...,
]
```

> The `route` argument should be a string or [`gettext_lazy()`](https://docs.djangoproject.com/en/4.2/ref/utils/#django.utils.translation.gettext_lazy "django.utils.translation.gettext_lazy") (see [Translating URL patterns](https://docs.djangoproject.com/en/4.2/topics/i18n/translation/#translating-urlpatterns)) that contains a URL pattern. The string may contain angle brackets (like `<username>` above) to capture part of the URL and send it as a keyword argument to the view

The `urlpatterns` list routes URLs to views. For more information please see:  
    https://docs.djangoproject.com/en/4.2/topics/http/urls/
	

# Examples:

## Function views

1. Add an import:  `from my_app import views`
2. Add a URL to urlpatterns:  `path('', views.home, name='home')`

## Class-based views

1. Add an import:  `from other_app.views import Home`
2. Add a URL to urlpatterns:  `path('', Home.as_view(), name='home')`

## Including another URLconf

1. Import the include() function: `from django.urls import include, path`
2. Add a URL to urlpatterns:  `path('blog/', include('blog.urls'))`

그래서, [[django.urls.include(module, namespace)]]가 뭡니까?
