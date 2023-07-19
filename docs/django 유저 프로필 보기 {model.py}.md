---
description:
aliases: 
tags: 
created: 2023-06-02T13:46:57
updated: 2023-07-19T13:57:05
title: django 유저 프로필 보기 {model.py}
---
[[django Creating custom user model using AbstractUser in django-RestFramework]]

- [blogtutorial/issues/4 {GH}](https://github.com/ESTsoft-Book-Project/blogtutorial/issues/4)
- 선행조건이... 무려 커스텀 `User` 클래스를 만들기다... 이래야 게시글을 작성한 걸 볼 수 있다.
- [Custom User Model {blog}](https://learndjango.com/tutorials/django-custom-user-model)의 스텝을 따라가보자. | [이것도 참고하세요 {blog}](https://testdriven.io/blog/django-custom-user-model/)

- [?] `AUTH_USER_MODEL`을 `settings.py`에 박아넣었는데 뭔지 모르겠군 | [Substituting a custom `User` model {doc}](https://docs.djangoproject.com/en/4.2/topics/auth/customizing/#substituting-a-custom-user-model)

> Tell Django to use our custom user model instead of the built-in User model.

조졌다. `blog.Article`이 `users.User`에 의존하고 `users.User`가 `blog.Article`에 의존한다. 따라서 circular import 상황이 발생하여 `ImportError`가 발생하였다. ==> 생각나는 유일한 해결책은 `Article`을 분리하여 `User` -> `ArticleID` <- `Article` 이런거? DB 시간에 배웠던 정규화와 관련이 있는 것 같은데 아는게 없으니까 답답하군. [[Changing to a custom user model mid-project]]

그래서 일단 커스텀 `User` 안에 필드를 다 주석처리하고 `makemigrations` , `migrate` 명령을 했더니 아래와 같은 에러메시지가 발생하였다.

```shell
ValueError: The field admin.LogEntry.user was declared with a lazy reference to 'users.user', but app 'users' isn't installed.
The field blog.Article.author was declared with a lazy reference to 'users.user', but app 'users' isn't installed.
The field blog.Article.likes was declared with a lazy reference to 'users.user', but app 'users' isn't installed.
The field blog.Article_likes.user was declared with a lazy reference to 'users.user', but app 'users' isn't installed.
```

[DB Queries in Django {doc}](https://docs.djangoproject.com/en/4.2/topics/db/queries/)  
[[django dependency issue 발생 시 AUTH_USER_MODEL이 제대로 박혀있는지 확인하라]]

> Due to limitations of Django’s dynamic dependency feature for swappable models, the model referenced by **AUTH_USER_MODEL** must be created in the first migration of its app (usually called 0001_initial); otherwise, you’ll have dependency issues.

[[writing a manager for a custom user model {djangodoc}]]

[[no such table 문제 해결방법 {django}]] ==> 그래도 같은 문제가 발생한다... 살려줘

# 20230603의 최승현이 나타났다

- [django sqlite 파일을 그냥 지우면 돼](https://stackoverflow.com/questions/66733285/how-to-reset-django-database)

물론 아직 문제가 완전히 다 해결된 건 아니다. createsuperuser에서 이메일, 유저네임, 비번을 치니까 에러가 발생했다. ==> `kwarg` 사용하여 인자를 명시할 때 반드시 key값을 함께 넣어주자.
