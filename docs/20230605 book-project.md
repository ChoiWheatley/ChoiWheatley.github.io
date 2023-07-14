---
description:
aliases: 
tags: 
created: 2023-06-05T11:25:29
updated: 2023-07-11T15:21:10
title: 20230605 book-project
---
- [[20230603 book-project|<< 20230603]] [[20230607 book-project|20230607 >>]]
	- [[postman]]사용방법 익혀야하지

[[python venv 환경설정]] 작성했다.

# TODO

- [x] bootstore 리포에 `README.md` 작성 + PR까지 => 초기세팅법에 대하여 + PR 하는 방법 알려주기
- [x] [[django model]] | 모델 이해도 높이고 커스텀 유저 만들어보자.
- [x] 조금만 더 욕심을 내면 커스텀 유저를 가지고 GET, POST, PATCH api도 만들어서 [[postman]]으로 실험해보자.

# 발견한 것들

- django.view.View를 상속한 클래스가 `get` 메서드를 오버라이드 하면 내가 원하는 템플릿파일을 `render` 함수에 담아 리턴할 수 있다.
```python
class SignupView(View):
"""HTTP Method callbacks"""
	def get(self, request):
		form = SignupForm()
		return render(request=request, template_name="users/signup.html", context={"form": form})
```

- `form.is_valid()`가 계속 실패한다. 그래서 에러메시지를 출력하도록 만들었더니 템플릿에서 submit한 form들의 name과 model과 일치하지 않아 발생한 문제였다.

- `django.db.utils.IntegrityError: UNIQUE constraint failed: users_user.username` 나는 커스텀 `User`에서 `username` 대신에 `email`을 사용하도록 설정했는데, 왜 계속 `username`이 고유하지 않다고 하는거지?
- 그래서 어쩔 수 없지만 [[AbstractBaseUser]]를 상속하여 많은 것들을 직접 구현하는 쪽으로 진행해 보아야 할 것 같다... AbstractUser는 일단 `username`이 존재하고, 이것을 `email`을 사용하도록 임의로 선회하는 것이고. | [[Store informations related to User, but not authorization-related {django}]] 

[[logging in django]]

- [?] PR 수락은 만장일치? Owner에게만? ==> 일단 최소 한 명 이상은 지정해줘야 하는 제약조건을 걸었다.

- [[protected branch에서 PR을 날리고 승인하는 방법]]
- 