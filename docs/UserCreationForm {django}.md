---
description:
aliases: 
tags: 
created: 2023-06-01T13:37:12
updated: 2023-07-11T15:21:07
title: UserCreationForm {django}
---
- [[django forms]]
- [Using the Django authentication system {doc}](https://docs.djangoproject.com/en/4.2/topics/auth/default/)
	- [models.User {doc}](https://docs.djangoproject.com/en/4.2/ref/contrib/auth/#django.contrib.auth.models.User)
	- 장고의 인증(authentication) 시스템은 인가(authorization)시스템과 결합되어있다. `User` 객체는 확장이 용이한 기본적인 형태의 인증 시스템으로, 회원가입, 로그인 절차와 밀접한 연관을 갖고 있다.
	- username, password, email, first_name, last_name 외에도 많은 attribute들이 있지만 걔네들은 다 옵셔널임.
- [forms.UserCreationForm {doc}](https://docs.djangoproject.com/en/4.1/topics/auth/default/#django.contrib.auth.forms.UserCreationForm) | [[django forms]]
	- 세개의 필드를 가지고 있다. username, password1, password2
	- 추가하고 싶은 필드가 있으면... 뭐 그냥 상속하면 된다. 그래서 아래 코드의 `UserRegisterForm`의 필드로 `email`이 들어있는거다.
		- 심지어 email 필드에 대한 기본정의조차 장고에서 제공해준다... | `forms.EmailField`

[[0014.1 Django 🎈]]에서 회원가입 폼을 만들 때 사용한 클래스를 한 번 보자. 이메일은 기본으로 제공이 안되어 내가 추가한 걸 볼 수 있는데, `Meta` 얘는 도대체 뭐냐?

```python
from django import forms
from django.contrib.auth.models import User
from django.contrib.auth.forms import UserCreationForm


class UserRegisterForm(UserCreationForm):
    """pre-built user registration forms.... SICK!"""
    email = forms.EmailField()

    class Meta:
        """details"""
        model = User
        fields = ['username', 'email', 'password1', 'password2']
```

![[django internal class Meta]]