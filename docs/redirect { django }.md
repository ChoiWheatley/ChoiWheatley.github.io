---
description:
aliases: 
tags: 
created: 2023-05-31T17:12:41
updated: 2023-07-15T21:33:03
title: redirect { django }
---
- [The Ultimate Guide to Django Redirects {Real Python}](https://realpython.com/django-redirects/)
- [django.shortcuts.redirect {doc}](https://docs.djangoproject.com/en/4.2/topics/http/shortcuts/#redirect)
---

# Why redirection?

- 로그인이 필요한 자원에 접근할 때 명시적으로 로그인 버튼을 누르게 만드는 건 사용자 피로도를 증가시킨다. → 따라서 자동으로 로그인 페이지로 리디렉션을 수행하고 로그인 완료시 본래 페이지로 되돌아가는 방식으로 작동한다면 매끄러울 것이다.
- 위와 비슷하지만 인가와도 관련이 있을 수 있다. 사용자 권한을 미리 알아낸 뒤 서로 다른 페이지로 리디렉션 할 수도 있다.
- bit.ly와 같이 자주 접근하지만 쓸데없이 긴 URL을 짧게 만든 뒤에 사용자들이 간편하게 링크를 들고다닐 수 있게
- 객체를 수정하거나 제거, 생성하는 등의 부가효과(side effects)를 일으키는 작업을 수행한 뒤에 리디렉션을 통해 사용자가 실수로 같은 작업을 두 번 일으키지 못하게 막을 수도 있다.

# Under the hood of redirection { 302 Found } { 301 Moved Permanently }

일반적인 GET 요청으로 리디렉션 URL을 요청한다면 [302 FOUND](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/302) 응답을 돌려받게 된다. 302는 애초에 redirection을 위한 상태코드이다. 헤더에 `Location`이 반드시 포함돼있고, 상대 URL을 따라 리디렉션을 수행할지말지를 클라이언트 단에서 결정할 수 있다.

> [!note] Redirection은 의무가 아니다

*Permanent Redirection*도 존재한다. [301 Moved Permanently](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/301)는 아예 URL을 이주한 경우 사용된다. 302와 마찬가지로 `Location` 헤더가 존재하여 부드럽게 새 주소로 이전하게 만들 수 있다.

# `HttpResponseRedirect`

`django.shortcuts.redirect()` 함수가 리턴하는 객체의 타입이다. 저 안에 기본적으로 302 코드와 Location 헤더를 담고 있다.

# `HttpResponsePermanentRedirect`

`django.shortcuts.redirect()` 함수의 kwargs 중 `permanent=True`를 추가하면 리턴하는 객체의 타입이다.
