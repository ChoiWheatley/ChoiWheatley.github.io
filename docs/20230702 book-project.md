---
aliases: 
tags: 
description:
created: 2023-07-02T17:58:39
updated: 2023-07-15T21:30:21
title: 20230702 book-project
---
- 어제 하던거: ![[20230701 book-project#^w5jgh9]]
- [[aws s3 static files in django]] 마저 진행.
- media 파일들은 https://allbooks-choi-2.s3.amazonaws.com/media/product_images/asdf-1 이런 식으로 잘 들어오거든. 그런데 JS는 무지성으로 http://127.0.0.1:8000/static/js/getCookie.js 이렇게 로컬에 있는 정적파일을 가져온다. 무엇이 문제인가?
- EC2 docker compose로 빌드한 웹앱 중[[kakaopay {django}]] 에서 발생한 에러

```
bookstore-django-1  | Internal Server Error: /purchases/kakaopay_start/
bookstore-django-1  | Traceback (most recent call last):
bookstore-django-1  |   File "/opt/venv/lib/python3.10/site-packages/django/core/handlers/exception.py", line 55, in inner
bookstore-django-1  |     response = get_response(request)
bookstore-django-1  |   File "/opt/venv/lib/python3.10/site-packages/django/core/handlers/base.py", line 197, in _get_response
bookstore-django-1  |     response = wrapped_callback(request, *callback_args, **callback_kwargs)
bookstore-django-1  |   File "/opt/venv/lib/python3.10/site-packages/django/contrib/auth/decorators.py", line 23, in _wrapper_view
bookstore-django-1  |     return view_func(request, *args, **kwargs)
bookstore-django-1  |   File "/app/purchases/views.py", line 190, in kakaopay_start
bookstore-django-1  |     purchase.kakaopay_checkout_tid = result['tid']
bookstore-django-1  | KeyError: 'tid'
```

- 진행한 저시기
	- https://developers.kakao.com/console 에 들어가 자기 앱 > 플랫폼 > Web  사이트 도메인에 포트를 포함한 IP 주소를 작성하면 된다.
