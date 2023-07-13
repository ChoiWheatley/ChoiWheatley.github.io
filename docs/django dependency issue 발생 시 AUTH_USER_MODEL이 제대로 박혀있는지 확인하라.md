---
description:
aliases: 
tags: 
created: 2023-06-02T22:01:42
updated: 2023-07-11T15:21:09
title: django dependency issue 발생 시 AUTH_USER_MODEL이 제대로 박혀있는지 확인하라
---
- https://dontrepeatyourself.org/post/django-custom-user-model-extending-abstractuser/
	- 에 따르면, 커스텀 user model을 만들때 settings.py에서 제목에서 말한 전역변수를 설정해 주어야 한다.
- [ValueError: The field admin.LogEntry.user was declared with a lazy reference {SOF}](https://stackoverflow.com/questions/50324561/valueerror-the-field-admin-logentry-user-was-declared-with-a-lazy-reference/60060092#60060092)
	- 에 따르면, AUTH_USER_MODEL을 수정한 채로 migrate를 할 때 문제가 발생할 수 있다고...
	- 그래서 앱 폴더 아래에 있는 `migrations` 폴더를 지운 뒤에 `makemigrations <app>`를 치면 된다.