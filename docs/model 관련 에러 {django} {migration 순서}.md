---
aliases: 
tags: 
description:
title: model 관련 에러 {django} {migration 순서}
created: 2023-07-19T13:49:49
updated: 2023-07-19T13:49:51
---

- 아직도 `users-model` 브랜치에서 에러를 고치지 못했다. 원인을 파악하자.
	- `forms.py`와 `admin.py`에서 기본 User를 전제로 생각해서 `username`을 참조하는 것 같다. 에러메시지를 요약하자면 다음과 같다.

	```
  File "/home/chltm/workspace/bookstore/venv/lib/python3.11/site-packages/django/forms/models.py", line 321, in __new__
	raise FieldError(message)
	django.core.exceptions.FieldError: Unknown field(s) (username) specified for User
	```

	- [[커스텀한 `User`의 `UserCreationForm` 재정의하기 {django}]]. 디폴트 User의 멤버를 기대하고 있기 때문이다. 그것은 바로 `username`. |
		- `forms.py`
		- `admin.py`
- migration 순서가 꼬이면 `raise InconsistentMigrationHistory`가 발생한다. `****.0001_initial` 을 의존하는 다른 `&&&&.0001_initial`가 존재하기 전에 migrate를 해서 그렇다. 이 문제를 해소하기 위해선 일단 `db.sqlite` 파일을 백업하던가 폭파시키던가 하고, 꼬인 앱의 `migration`를 지운다. 그리고 `makemigrations` → `migrate` 명령을 수행하면 된다. | [[django migration 순서 꼬인경우 {GPT}]]
- [[django reverse]]
