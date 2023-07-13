---
description:
aliases: 
tags: 
created: 2023-05-29T16:24:09
updated: 2023-07-11T15:21:08
title: django urlpatterns and url convert
---
- [url pattern DJANGODOC -- path converters](https://docs.djangoproject.com/en/3.0/topics/http/urls/#path-converters)
- [How django processes a request DJANGODOC](https://docs.djangoproject.com/en/3.0/topics/http/urls/#how-django-processes-a-request)

When a user requests a page from your Django-powered site, this is the algorithm the system follows to determine which Python code to execute:

1. Django determines the root URLconf module to use. Ordinarily, this is the value of the [`ROOT_URLCONF`](https://docs.djangoproject.com/en/3.0/ref/settings/#std:setting-ROOT_URLCONF) setting, but if the incoming `HttpRequest` object has a [`urlconf`](https://docs.djangoproject.com/en/3.0/ref/request-response/#django.http.HttpRequest.urlconf "django.http.HttpRequest.urlconf") attribute (set by middleware), its value will be used in place of the [`ROOT_URLCONF`](https://docs.djangoproject.com/en/3.0/ref/settings/#std:setting-ROOT_URLCONF) setting.
    
2. Django loads that Python module and looks for the variable `urlpatterns`. This should be a [sequence](https://docs.python.org/3/glossary.html#term-sequence "(in Python v3.9)") of [`django.urls.path()`](https://docs.djangoproject.com/en/3.0/ref/urls/#django.urls.path "django.urls.path") and/or [`django.urls.re_path()`](https://docs.djangoproject.com/en/3.0/ref/urls/#django.urls.re_path "django.urls.re_path") instances.
    
3. Django runs through each URL pattern, in order, and stops at the first one that matches the requested URL, matching against [`path_info`](https://docs.djangoproject.com/en/3.0/ref/request-response/#django.http.HttpRequest.path_info "django.http.HttpRequest.path_info").
    
4. Once one of the URL patterns matches, Django imports and calls the given view, which is a Python function (or a [class-based view](https://docs.djangoproject.com/en/3.0/topics/class-based-views/)). The view gets passed the following arguments:
    
    - An instance of [`HttpRequest`](https://docs.djangoproject.com/en/3.0/ref/request-response/#django.http.HttpRequest "django.http.HttpRequest").
        
    - If the matched URL pattern contained no named groups, then the matches from the regular expression are provided as positional arguments.
        
    - The keyword arguments are made up of any named parts matched by the path expression that are provided, overridden by any arguments specified in the optional `kwargs` argument to [`django.urls.path()`](https://docs.djangoproject.com/en/3.0/ref/urls/#django.urls.path "django.urls.path") or [`django.urls.re_path()`](https://docs.djangoproject.com/en/3.0/ref/urls/#django.urls.re_path "django.urls.re_path").
        
        Changed in Django 3.0:
        
        In older versions, the keyword arguments with `None` values are made up also for not provided named parts.
        
5. If no URL pattern matches, or if an exception is raised during any point in this process, Django invokes an appropriate error-handling view. See [Error handling](https://docs.djangoproject.com/en/3.0/topics/http/urls/#error-handling) below.