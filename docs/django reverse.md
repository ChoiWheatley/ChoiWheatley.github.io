---
description:
aliases: 
tags: 
created: 2023-06-07T14:11:36
updated: 2023-07-19T01:08:52
title: django reverse
---
- 통째로 URL을 다 적지 않고도 말 그대로 **거꾸로** URL을 추적하는 편리한 기능 
- `redirect(reverse("users:signup"))`: 엔드포인트를 추적한다.
- `redirect("users:signup")`: URL 패턴 이름을 사용하여 리디렉션.
- reverse를 가능하게 하기 위해선 필요한 조건이 있었는데 뭐더라? 전역변수에 뭘 할당했던 것 같은데

# No Reverse Match 

에러가 나왔을 때 해결해야 할 것들에 대해 정리해보자. 

```
django.urls.exceptions.NoReverseMatch: Reverse for 'signin' not found. 'signin' is not a valid view function or pattern name.
```

1. 템플릿을 제대로 작성했나?
	1. `{% url '<app-name>:<url-name>'%}`을 제대로 작성했나? 실수로 `<app-name>/<url-name>` 이런 식으로 쓰거나 하면 위의 에러가 발생한다.
2. urls.py를 코어에서 제대로 연결했나?
	1. 코어 `urlpatterns`에 명시가 제대로 됐는지 확인하라.
	2. 특히, `include` 문법을 사용할 땐 해당 앱의 `urls.py` 전역변수에 `app_name`을 지정해 주어야 한다. | [[django.urls.include(module, namespace)]]
3. `reverse`의 인자를 제대로 작성했나? | [[django reverse]]
	1. reverse 인자도 마찬가지로 `:` 구분자를 사용한다는거.
