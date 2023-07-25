---
description:
aliases: 
tags: 
created: 2023-06-01T17:01:51
updated: 2023-07-25T11:22:05
title: django authorization for accessing인가 {mixins.{LoginRequiredMixin, UserPassesTestMixin}}
---
- [User authentication in django {doc}](https://docs.djangoproject.com/en/4.2/topics/auth/)
	- [`LoginRequiredMixin` {doc}](https://docs.djangoproject.com/en/4.2/topics/auth/default/#the-loginrequiredmixin-mixin)
	- [`UserPassesTestMixin` {doc}](https://docs.djangoproject.com/en/4.2/topics/auth/default/#django.contrib.auth.mixins.UserPassesTestMixin)
- [Delete Article {YT}](https://youtu.be/sMqDJovFO-Y?t=7364)
- [[0014.1 Django 🎈]] 에서 아티클을 지우는 페이지로 가는 GET요청과 실제 삭제를 수행하는 POST요청을 현재 아무나 할 수 있게 되었다. 
	- [delete_article.html {GH}](https://github.com/ChoiWheatley/blogtutorial/blob/4a8ae28064ce08be811510b5040d556e41cfe369/blog/templates/blog/delete_article.html)
	- [views.py {GH}](https://github.com/ChoiWheatley/blogtutorial/blob/4a8ae28064ce08be811510b5040d556e41cfe369/blog/views.py)
- 하지만 이것은 우리가 원하던 유즈케이스가 아니다. 따라서 현재 세션에 로그인한 사람과 삭제하고자 하는 아티클의 author.id가 일치하는 경우에만 `<int:pk>/delete` 라우트에 접근할 수 있도록 ==인가==해야 할 것이다.
- `django.contrib.auth.mixins.LoginRequiredMixin` 클래스를 상속받은 View는 자동적으로 User가 로그인된 상태인지를 확인하게 된다. 만약 로그인 상태가 아니라면 404를 띄워준다.
- 만약 404 대신에 로그인 페이지로 넘어가고 싶다면 `settings.py` 안에 `LOGIN_REDIRECT_URL` 전역변수를 채워주면 된다. | [LOGIN_REDIRECT_URL {doc}](https://docs.djangoproject.com/en/4.1/ref/settings/#login-redirect-url) 
	- The URL or [named URL pattern](https://docs.djangoproject.com/en/4.1/topics/http/urls/#naming-url-patterns) where requests are redirected after login when the [`LoginView`](https://docs.djangoproject.com/en/4.1/topics/auth/default/#django.contrib.auth.views.LoginView "django.contrib.auth.views.LoginView") doesn’t get a `next` GET parameter.
- 하지만 위의 방식만 사용하면 눈치빠른 유저는 로그인을 해놓고 다른 유저의 아티클을 지울 우려가 있다. `LoginRequiredMixin`은 그냥 단지 로그인 여부만을 확인하기 때문이다. 따라서 `UserPassesTestMixin`을 추가로 상속받아 악의적인 URL 접근이 들어왔을 때 접근불가 403 에러를 띄워야 한다.
	- `test_func` 메서드를 오버라이딩 해야하는군.
- [수정본 {GH}](https://github.com/ChoiWheatley/blogtutorial/blob/9c077b3fb340db4496dd289687c642f33239b55b/blog/views.py)

## `redirect_field_name`

`LoginRequiredMixin`을 상속한 클래스는 해당 필드를 정의하여 로그인 이후에 어디로 갈건지를 정의할 수 있다.
