---
description:
aliases: 
tags: 
created: 2023-06-07T10:31:28
updated: 2023-07-11T15:21:10
title: 20230607 book-project
---
- [?] `templates` 폴더를 밖으로 빼내야 하는 이유: static 파일들을 별도의 폴더에 관리하기 위해서. ==> 그렇다면 장고 라이브러리는 왜 밖으로 빼내지 않았나?

- [[django PermissionsMixin의 역할이 뭐지]]

# Model 관련 에러
- 아직도 `users-model` 브랜치에서 에러를 고치지 못했다. 원인을 파악하자.
	- `forms.py`와 `admin.py`에서 기본 User를 전제로 생각해서 `username`을 참조하는 것 같다. 에러메시지를 요약하자면 다음과 같다.
	```
  File "/home/chltm/workspace/bookstore/venv/lib/python3.11/site-packages/django/forms/models.py", line 321, in __new__
	raise FieldError(message)
	django.core.exceptions.FieldError: Unknown field(s) (username) specified for User
	```
	- [[`User`를 커스텀 하고싶으면, `User`말고도 다른 부분의 코드도 함께 변경해야 한다]]. 디폴트 User의 멤버를 기대하고 있기 때문이다. 그것은 바로 `username`. |
		- `forms.py`
		- `admin.py`
- migration 순서가 꼬이면 `raise InconsistentMigrationHistory`가 발생한다. `****.0001_initial` 을 의존하는 다른 `&&&&.0001_initial`가 존재하기 전에 migrate를 해서 그렇다. 이 문제를 해소하기 위해선 일단 `db.sqlite` 파일을 백업하던가 폭파시키던가 하고, 꼬인 앱의 `migration`를 지운다. 그리고 `makemigrations` → `migrate` 명령을 수행하면 된다. | [[django migration 순서 꼬인경우 {GPT}]]
- [[django reverse]]