---
aliases: 
tags: 
description:
title: Filtering {drf}
created: 2024-11-22T22:41:35
updated: 2024-11-22T22:56:35
---
- ref: <https://www.django-rest-framework.org/api-guide/filtering>

DRF에서 필터링은 [[generic View {drf}|generic view]] Class의 `get_queryset` 메서드를 오버라이드 하는 것으로 충분히 달성할 수 있다. 

## Filtering Against Query Parameters

- ref: <https://www.django-rest-framework.org/api-guide/filtering/#filtering-against-query-parameters>

`get_queryset` 메서드를 오버라이드 하여 쿼리셋에 `request.query_params` 를 가져와 원하는 필터링을 수행할 수 있다. 예제 코드는 공식문서에서 가져왔다:

```python
class PurchaseList(generics.ListAPIView):
    serializer_class = PurchaseSerializer

    def get_queryset(self):
        """
        Optionally restricts the returned purchases to a given user,
        by filtering against a `username` query parameter in the URL.
        """
        queryset = Purchase.objects.all()
        username = self.request.query_params.get('username')
        if username is not None:
            queryset = queryset.filter(purchaser__username=username)
        return queryset
```
