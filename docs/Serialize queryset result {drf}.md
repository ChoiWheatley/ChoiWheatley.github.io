---
aliases: 
tags: 
description:
title: Serialize queryset result {drf}
created: 2024-11-30T14:12:23
updated: 2024-11-30T14:27:10
---
- ref: 
	- Github Issue (closed): <https://github.com/encode/django-rest-framework/issues/2709>
	- Accessing the initial data and instance: <https://www.django-rest-framework.org/api-guide/serializers/#accessing-the-initial-data-and-instance>
---

## 문제 확인

쿼리셋 결과를 직렬화하기 위해 아래와 같은 코드를 작성했다:

```python
serializer = self.serializer_class(data=queryset, many=True)
```

테스트 결과에 에러가 발생했다:

```
AssertionError: When a serializer is passed a `data` keyword argument you must call `.is_valid()` before attempting to access the serialized `.data` representation.
You should either call `.is_valid()` first, or access `.initial_data` instead.
```

## 원인

[serializers.py #L112](https://github.com/encode/django-rest-framework/blob/dbac145638758413b966c3418fa5f3f651e3e02a/rest_framework/serializers.py#L112)를 살펴보자:

```python
    def __init__(self, instance=None, data=empty, **kwargs):
        self.instance = instance
        if data is not empty:
            self.initial_data = data
        self.partial = kwargs.pop('partial', False)
        self._context = kwargs.pop('context', {})
        kwargs.pop('many', None)
        super().__init__(**kwargs)
```

Serializer 클래스의 생성자의 kwargs가 다른 이름으로 생성을 시도했다! 

> [!question] 그렇다면 `instance` 와 `data`의 차이점은 무엇이지?

[Accessing the initial data and instance](https://www.django-rest-framework.org/api-guide/serializers/#accessing-the-initial-data-and-instance) 에 따르면, 직렬화를 시도하는 재료인 queryset은 `instance`를 통해 넣고 역직렬화를 시도하는 재료인 request.data는 `data`를 통해 넣어야 한다고 한다.

## 해결

`instance=` kwarg를 사용하거나 어차피 첫번째 positional arg이므로 굳이 키를 적지 않아도 된다.

```python
serializer = self.serializer_class(instance=queryset, many=True)
```
