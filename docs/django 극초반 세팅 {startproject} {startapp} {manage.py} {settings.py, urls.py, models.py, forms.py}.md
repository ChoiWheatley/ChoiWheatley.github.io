---
aliases: 
tags: 
description:
created: 2023-07-06T13:44:57
updated: 2023-07-18T17:56:02
title: django 극초반 세팅 {startproject} {startapp} {manage.py} {settings.py, urls.py, models.py, forms.py}
---

# 프로젝트 생성 및 app 추가

- `django` 패키지 설치 및 초기 프로젝트 마련 | `django-admin startproject <project-name>` 
- `app` 을 만들어 다양한 유즈케이스에 대응하기  (아래는 장고 블로그 튜토리얼 예시임.)
	- `python manage.py startapp blog` 
	- `python manage.py startapp users`
	- `blog` : 블로그 아티클 관리, 유저관리, 좋아요, 게시글 삭제, 게시글 상세보기
	- `users` : 로그인 로그아웃 회원가입, forms 관리, 
	- `blogtutorial` : admin 전용 유즈케이스 모음
- Secret Key 및 각종 민감한 데이터를 숨기기 위하여 [`python-decouple`](https://github.com/HBNetwork/python-decouple) 모듈을 사용한다.

	- specify .env path

	```python
	config = Config(RepositoryEnv("path/to/.env"))
	```

	- use it with django

	```python
	DEBUG = config("DEBUG", default=False, cast=bool)
	```

	- 주로 저장하는 데이터들
		- `DEBUG`
		- `SECRET_KEY`
		- `DATABASE_URL`: 커스텀 DB 사용시

# 각 파일의 목적

- **`settings.py`**: 전역상수들을 정의하여 장고 프로젝트의 세팅을 관할하는 곳. 
	- `INSTALLED_APPS`
	- `BASE_DIR`
	- `SECRET_KEY`
	- `DEBUG`
	- `INSTALLED_APPS`
	- `MIDDLEWARE` [cors 관련 미들웨어 {GH PR}](https://github.com/ESTsoft-Book-Project/bookstore/pull/137)
	- `SITE_ID` [Site matching query does not exist {GH issue}](https://github.com/ESTsoft-Book-Project/bookstore/issues/63#issuecomment-1610864033)
	- `DATABASES`
	- `STATIC_URL`
	- `STATIC_ROOT`
	- `MEDIA_URL`
	- `MEDIA_ROOT`
	- `LOGIN_URL`
- **`urls.py`** : `blog`, `accounts`, `admin` 페이지로 이동할 수 있는 관문.
	- `urls.py`에 라우트 주소(`route`)와 view 객체(콜백함수 또는 `View.as_view()`), 템플릿에서 사용할 명칭(`name`) 등에 대하여 명시한다. 
	- [[pk { urlpatterns { path } } {django}]]
- **`views.py`**: `urls.py`에서 정의한 `path`들에 명시한 view 함수들의 모음. 클래스를 활용하는 방법과 함수를 활용하는 방법을 선택할 수 있다.
	- `views.py`에 `django.views.View`를 상속하는 클래스를 만들어 GET, POST 등과 같은 HTTP 메서드 요청에 따른 콜백함수를 다루는 함수 `get, post` 등을 오버라이드 한다.
	- [[django.views.{View, generic.{ListView, DetailView}}]]
- **`models.py`** 에 데이터를 명시하는 필드를 만들어 정의한다. SQL 문을 단 하나도 알지 못해도 작성할 수 있는 데이터 모델링~~~ 
	- `User` 라는 모델이 디폴트로 이미 주어지기 때문에 `users` 앱은 단지 회원가입, 로그인, 로그아웃 관련한 유즈케이스만 다룬다. 
	- [[django model]] 
- **`forms.py`**: 모델을 생성하고, 수정하는 코드를 편하게 작성하고 유효성 검증도 해주는 클래스를 제공.
	- [[django forms]]

# manage.py 명령어

- [[django migrate]]를 해 준다. | `python manage.py migrate`
- admin 계정 생성 | `python manage.py createsuperuser`
- `python manage.py runserver`
