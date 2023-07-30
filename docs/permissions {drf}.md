---
aliases: 
tags: 
description:
title: permissions {drf}
created: 2023-07-31T07:14:55
updated: 2023-07-31T07:23:21
---
- [doc](https://www.django-rest-framework.org/api-guide/permissions/)

system-wide permission policy: `settings.py` 안에 `REST_FRAMEWORK`를 설정

```python
REST_FRAMEWORK = {
	"DEFAULT_PERMISSION_CLASSES": [
		"rest_framework.permissions.IsAuthenticated",
	]
}
```

`APIView` 클래스 단위의 permission policy: `permission_classes` 속성을 사용

```python
from rest_framework.permissions import IsAuthenticated
from rest_framework.response import Response
from rest_framework.views import APIView

class ExampleView(APIView):
    permission_classes = [IsAuthenticated]

    def get(self, request, format=None):
        content = {
            'status': 'request was permitted'
        }
        return Response(content)
```

함수 단위의 permission policy: `api_view` 데코레이터 사용

```python
from rest_framework.decorators import api_view, permission_classes
from rest_framework.permissions import IsAuthenticated
from rest_framework.response import Response

@api_view(['GET'])
@permission_classes([IsAuthenticated])
def example_view(request, format=None):
    content = {
        'status': 'request was permitted'
    }
    return Response(content)
```

객체 단위의 permission policy: `self.check_object_permissions` 메서드 사용. `get_object` 메서드 자체에 이미 권한확인 코드가 있다.

