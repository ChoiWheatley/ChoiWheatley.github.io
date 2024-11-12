---
aliases: 
tags: 
description:
title: extend_schema {drf_spectacular}
created: 2024-11-12T23:51:33
updated: 2024-11-13T00:00:42
---
- ref: 
	- <https://drf-spectacular.readthedocs.io/en/latest/customization.html>
	- <https://drf-spectacular.readthedocs.io/en/latest/drf_spectacular.html#drf_spectacular.utils.extend_schema>
---
summary, example, respone, request, parameter를 정의해서 OpenAPI 문서의 내용을 추가하기 위해 사용되는 데코레이터 함수이다.

예시:

```python
class CalendarCreateView(APIView):
    @extend_schema(
        summary="캘린더 생성",
        description="새 캘린더를 생성합니다.",
        request=CalendarCreateSerializer,
        responses={201: CalendarDetailSerializer},
        tags=["Calendars"],
    )
    def post(self, request):
        # Placeholder implementation
        return Response({"message": "Calendar created"}, status=status.HTTP_201_CREATED)
```

> request, response, parameter에 Serializer 클래스 또는 Serializer 인스턴스가 들어갈 수 있다.

Serializer 인스턴스가 들어가는 경우는 Serializer에 `many=True` 옵션과 같은 경우에 대응할 수 있게된다.

예시:

```python
class CalendarListView(ListAPIView):
    @extend_schema(
        summary="캘린더 목록 조회",
        description="유저가 등록한 모든 캘린더를 불러옵니다.",
        responses={200: CalendarDetailSerializer(many=True)},
        tags=["Calendars"],
    )
    def get(self, request):
        pass
```

예시 Swagger Response:

```json
[
  {
    "id": 0,
    "user": "string",
    "created_at": "2024-11-12T14:36:54.956Z",
    "updated_at": "2024-11-12T14:36:54.956Z",
    "title": "string"
  }
]
```
