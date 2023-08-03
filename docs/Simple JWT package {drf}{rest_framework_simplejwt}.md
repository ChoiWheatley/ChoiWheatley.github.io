---
aliases: 
tags: 
description: JSON Web Token plugin for the Django REST Framework
title: Simple JWT package {drf}{rest_framework_simplejwt}
created: 2023-08-02T14:01:47
updated: 2023-08-04T00:36:24
---
- [doc](https://django-rest-framework-simplejwt.readthedocs.io/en/latest/getting_started.html)
- [예제 {YT}](https://youtu.be/AfYfvjP1hK8?t=1228)
- parent links:
	- [[DRF에서 인증기능 만들기 {drf}]]
___

# settings

[공식문서](https://django-rest-framework-simplejwt.readthedocs.io/en/latest/settings.html)에서 가져옴.

`settings.py`

```python
# Django project settings.py

from datetime import timedelta


SIMPLE_JWT = {
    "ACCESS_TOKEN_LIFETIME": timedelta(minutes=5),
    "REFRESH_TOKEN_LIFETIME": timedelta(days=1),
    "ROTATE_REFRESH_TOKENS": False,
    "BLACKLIST_AFTER_ROTATION": False,
    "UPDATE_LAST_LOGIN": False,

    "ALGORITHM": "HS256",
    "SIGNING_KEY": settings.SECRET_KEY,
    "VERIFYING_KEY": "",
    "AUDIENCE": None,
    "ISSUER": None,
    "JSON_ENCODER": None,
    "JWK_URL": None,
    "LEEWAY": 0,

    "AUTH_HEADER_TYPES": ("Bearer",),
    "AUTH_HEADER_NAME": "HTTP_AUTHORIZATION",
    "USER_ID_FIELD": "id",
    "USER_ID_CLAIM": "user_id",
    "USER_AUTHENTICATION_RULE": "rest_framework_simplejwt.authentication.default_user_authentication_rule",

    "AUTH_TOKEN_CLASSES": ("rest_framework_simplejwt.tokens.AccessToken",),
    "TOKEN_TYPE_CLAIM": "token_type",
    "TOKEN_USER_CLASS": "rest_framework_simplejwt.models.TokenUser",

    "JTI_CLAIM": "jti",

    "SLIDING_TOKEN_REFRESH_EXP_CLAIM": "refresh_exp",
    "SLIDING_TOKEN_LIFETIME": timedelta(minutes=5),
    "SLIDING_TOKEN_REFRESH_LIFETIME": timedelta(days=1),

    "TOKEN_OBTAIN_SERIALIZER": "rest_framework_simplejwt.serializers.TokenObtainPairSerializer",
    "TOKEN_REFRESH_SERIALIZER": "rest_framework_simplejwt.serializers.TokenRefreshSerializer",
    "TOKEN_VERIFY_SERIALIZER": "rest_framework_simplejwt.serializers.TokenVerifySerializer",
    "TOKEN_BLACKLIST_SERIALIZER": "rest_framework_simplejwt.serializers.TokenBlacklistSerializer",
    "SLIDING_TOKEN_OBTAIN_SERIALIZER": "rest_framework_simplejwt.serializers.TokenObtainSlidingSerializer",
    "SLIDING_TOKEN_REFRESH_SERIALIZER": "rest_framework_simplejwt.serializers.TokenRefreshSlidingSerializer",
}
```

# postman에서 확인

[usage{doc}](https://django-rest-framework-simplejwt.readthedocs.io/en/latest/getting_started.html#usage)  

회원 이메일과 비밀번호를 가지고 JWT를 인코딩한 결과이다. 

![[Pasted image 20230802142521.png]]  

장고 인증서버에게 유저 아이디와 비밀번호를 사용하여 access token, refresh token 모두를 받아냈다. 이제 이것으로 장고 리소스 서버에게 인가를 요구할 수 있다.

access token을 가지고 <https://jwt.io> 에 들어가 바로 디버깅을 실시해보자.  

![[Pasted image 20230802142807.png]]

access token을 사용하여 특정한 view에 접근하는 방식에 대한 스니펫이다.

```shell
curl \
  -H "Authorization: Bearer {{access-token}}" \
  http://localhost:8000/api/some-protected-view/
```

그러면 이 `some-protected-view`를 어떻게 만드는 건지 궁금해진다.

### access

`core.views.ExampleView`:

```python
class ExampleView(views.APIView):
    """
    sample code from drf documentation
    ref: https://www.django-rest-framework.org/api-guide/authentication/#setting-the-authentication-scheme
    """

    # authentication_classes = [TokenAuthentication]
    permission_classes = [IsAuthenticated]

    def get(self, request, format=None):
        content = {
            "user": str(request.user),
            "auth": str(request.auth),
        }
        return Response(content)
```

이유는 아직 잘 모르겠는데, `authentication_classes`가 있으니까 자격증명이 실패했다. 따라서 `permission_classes`만 있으면 될 것 같다.

![[Pasted image 20230802164348.png]]

expired 상태의 토큰을 access 했을 때 돌아온 응답: `401-unauthorized`

```json
{
    "detail": "이 토큰은 모든 타입의 토큰에 대해 유효하지 않습니다",
    "code": "token_not_valid",
    "messages": [
        {
            "token_class": "AccessToken",
            "token_type": "access",
            "message": "유효하지 않거나 만료된 토큰"
        }
    ]
}
```

401이 왔을때 자동으로 토큰을 refresh 하도록 만들어 주어야 한다.

### refresh

만약 세팅에서 `ROTATE_REFRESH_TOKENS`를 켜두면 refresh 마다 access, refresh 토큰이 모두 재발급 된다.

![[Pasted image 20230802155350.png]]

# 아니, 디코딩은 어떻게 하냐?

다 방법이 있지. [다음 문서](https://django-rest-framework-simplejwt.readthedocs.io/en/latest/rest_framework_simplejwt.html#rest_framework_simplejwt.authentication.JWTAuthentication.authenticate)를 읽어봐라. [스택오버플로](https://stackoverflow.com/a/68342977/21369350)도 읽어봐라. 

이제 `ExampleView`가 요청을 디코딩하여 각각 `user`, `token`으로 분해하는 코드를 보자.

```python
lass ExampleView(views.APIView):
    """
    sample code from drf documentation
    ref: https://www.django-rest-framework.org/api-guide/authentication/#setting-the-authentication-scheme
    """

    # authentication_classes = [TokenAuthentication]
    permission_classes = [IsAuthenticated]

    def get(self, request, format=None):
        authenticator = JWTAuthentication()
        response = authenticator.authenticate(request)
        if response:
            user, token = response
            ser_user = MemberSerializer(user)
            return Response(
                {
                    "user": ser_user.data,
                    "token": token.payload,
                }
            )
        return Response({"error": "cannot authenticate"})
```

그 결과는 다음과 같다.

```json
{
    "user": {
        "id": 5,
        "last_login": null,
        "email": "a@a.com",
        "nickname": "a",
        "is_staff": false,
        "is_superuser": false,
        "is_active": true,
        "born_year": null,
        "job": null
    },
    "token": {
        "token_type": "access",
        "exp": 1690964101,
        "iat": 1690961566,
        "jti": "8743ccc6ccfb437d937f39da6ad303c5",
        "user_id": 5
    }
}
```

만약 저 `permission_classes`가 `IsAdminUser`라면 admin access token만이 통과할 수 있겠지?

# [Creating tokens manually](https://django-rest-framework-simplejwt.readthedocs.io/en/latest/creating_tokens_manually.html)

회원가입 후에 자동으로 토큰을 발급하게 만들어주고 싶다.

```python
from rest_framework_simplejwt.tokens import RefreshToken

def get_tokens_for_user(user):
    refresh = RefreshToken.for_user(user)

    return {
        'refresh': str(refresh),
        'access': str(refresh.access_token),
    }
```
