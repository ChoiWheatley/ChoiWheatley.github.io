---
description:
aliases: 
tags: 
created: 2023-06-05T22:13:43
updated: 2023-07-19T10:41:58
title: Customizing authentication in {django} {AbstractUser, AbstractBaseUser}
---
- [full example {doc}](https://docs.djangoproject.com/en/dev/topics/auth/customizing/#a-full-example)
- [How to use email as username in django](https://dev.to/chokshiroshan/how-to-use-email-as-username-for-django-authentication-8if)
- [[커스텀한 `User`의 `UserCreationForm` 재정의하기 {django}]]
- [[django Creating custom user model using AbstractUser in django-RestFramework]]
___

# `AbstractBaseUser`

비밀번호 해싱에 필요한 구현체를 마련해놓고 프로그래머에게 기본 `User`를 확장할 수 있게 도와준다. 오버라이딩이 필요한 멤버는 다음과 같다. 
- `USERNAME_FIELD` => default authentication backend는 pk 아닌 최소 하나의 필드를 고유하게 만들어야 하는데, 그것이 `username` 필드이다. 다른 필드를 고유하게 만들고 싶다면 얘를 수정해주면 된다.
- `REQUIRED_FIELDS` => `createsuperuser` 명령시 이걸 보고 input field를 제공.
- `EMAIL_FIELD` (option) => `get_email_field_name()` 메서드가 리턴하는 녀석임.
- `is_active` (option)

다만, `AbstractBaseUser`는 `UserManager`라는 유저 생성에 관여하는 매니저 객체를 필요로 한다. 커스텀 `User`가 단지 필드만 수정한 경우라면 `BaseUserManager`를 상속하는 것이 유리하다. 오버라이드 할 메서드는 다음과 같다.
- `create_user(username_field, password, **other_fields)`
- `create_superuser(username_field, password, **other_fields)`

# `AbstractUser`

> [!info]  
> `AbstractUser` extends `AbstractBaseUser` with full implementation of the default `User` as an abstract model.

따라서, `AbstractUser`는 기본 `UserManager`또한 구현하고 있다는 것을 알 수 있다.
