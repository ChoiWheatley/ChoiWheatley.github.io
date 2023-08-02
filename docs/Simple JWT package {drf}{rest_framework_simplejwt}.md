---
aliases: 
tags: 
description: JSON Web Token plugin for the Django REST Framework
title: Simple JWT package {drf}{rest_framework_simplejwt}
created: 2023-08-02T14:01:47
updated: 2023-08-02T14:41:50
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

회원 이메일과 비밀번호를 가지고 JWT를 인코딩한 결과이다. 

![[Pasted image 20230802142521.png]]  

access token을 가지고 <https://jwt.io> 에 들어가 바로 디버깅을 실시해보자.  

![[Pasted image 20230802142807.png]]

장고 인증서버에게 유저 아이디와 비밀번호를 사용하여 access token, refresh token 모두를 받아냈다. 이제 이것으로 장고 리소스 서버에게 인가를 요구할 수 있다.
