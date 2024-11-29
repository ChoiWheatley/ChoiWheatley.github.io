---
aliases: 
tags: 
description:
title: generic View {drf}
created: 2024-11-22T23:00:43
updated: 2024-11-29T18:15:00
---
- ref: <https://www.django-rest-framework.org/api-guide/generic-views/>

Class Based Views의 장점은 반복을 줄일 수 있다는 점이다. 특히 리소스 CRUD에 대한 추상화 레이어를 제공하여 `queryset`과 `serializer_class`만 정의하면 기본적인 메서드인 `list`, `create`, `retrieve`, `update`, `partial_update`, `destroy`등 **액션 메서드**를 정의한 여러 Mixin 클래스들을 상속받아 세부 로직을 구현할 필요 없이 여러 **핸들러 메서드**들을 편하게 구현할 수 있다.

> [!question] GET, POST, PUT, PATCH, DELETE 메서드들과는 어떤 연관성을 정의할 수 있지? 

[rest_frameworks/generics.py](https://github.com/encode/django-rest-framework/blob/master/rest_framework/generics.py) 코드를 보니 Concrete View Class들인 `CreateAPIView`는 `post`를, `ListAPIView`는 `get`을, 등등 이렇게 매핑이 되어있다. 그러니까 우리가 generic api view를 사용할때엔 일반적인 `APIView` 클래스를 상속한 것과 같이 `get`, `post`, `put`, `delete` 같은 **핸들러** 메서드를 직접 구현해서 써야한다.

그래서 urls.py에 `APIView` 하듯이 등록하면 된다고:

```python
urlpatterns = [
    path('my-items/', MyListView.as_view(), name='my-list'),
    path('my-items/<int:pk>/', MyDetailView.as_view(), name='my-detail'),
]
```

Mixin을 쓰거나 [[ViewSets and routers {drf}]]을 사용할 경우, [drf / SimpleRouter](https://www.django-rest-framework.org/api-guide/routers/#simplerouter) 문서에 따르면 ViewSet에서 정의된 **액션**과 HTTP 메서드들 간의 매핑관계를 표현한다. generic api view에서도 믹스인을 사용할 수는 있으나 얘네들은 **액션**만을 제공한다는 점을 유의하라.

| URL Style                     | HTTP Method                                | Action                                   | URL Name              |
| ----------------------------- | ------------------------------------------ | ---------------------------------------- | --------------------- |
| {prefix}/                     | GET                                        | list                                     | {basename}-list       |
| {prefix}/                     | POST                                       | create                                   | {basename}-list       |
| {prefix}/{url_path}/          | GET, or as specified by `methods` argument | `@action(detail=False)` decorated method | {basename}-{url_name} |
| {prefix}/{lookup}/            | GET                                        | retrieve                                 | {basename}-detail     |
| {prefix}/{lookup}/            | PUT                                        | update                                   | {basename}-detail     |
| {prefix}/{lookup}/            | PATCH                                      | partial_update                           | {basename}-detail     |
| {prefix}/{lookup}/            | DELETE                                     | destroy                                  | {basename}-detail     |
| {prefix}/{lookup}/{url_path}/ | GET, or as specified by `methods` argument | `@action(detail=True)` decorated method  | {basename}-{url_name} |

## Basic settings

클래스에 속성을 추가하거나 메서드를 오버라이딩하여 기본 행동을 정의할 수 있다:

- `queryset` 속성을 추가하거나 `get_queryset` 메서드를 오버라이딩하여 이 뷰가 어떤 데이터에 접근해야 하는지에 대한 속성을 정의할 수 있다.
- `serializer_class` 속성을 추가하거나 `get_serializer_class` 메서드를 오버라이딩 하여 직렬화 / 역직렬화 / 유효성 검사를 수행할 수 있다.
- `lookup_field` 속성을 추가하면 `queryset`에 인스턴스를 쿼리할때 어떤 필드를 사용할 것인지를 정의할 수 있다. 기본값은 역시 `'pk'`이다.
- `pagination_class` 속성을 정의하여 페이지네이션에 대한 책임을 전가할 수 있다.
- `filter_backends` 속성을 정의하여 쿼리셋 필터링을 책임질 클래스를 지정할 수 있다.

## Methods

### `get_queryset(self)`

### `filter_queryset(self)`

항상 모든 데이터를 list하지 않는 경우를 위해서 `filter_queryset`이라는 메서드를 정의하여 queryset에 필터링 후크를 걸 수도 있다.

자세한 필터링에 관한 내용은 [[Filtering {drf}]] 을 확인할 것.

### `get_object(self)`

### `get_serializer_class(self)`

### `perform_create(self, serializer)` - `CreateModelMixin`

### `perform_update(self, serializer)` - `UpdateModelMixin`

### `perform_destroy(self, instance)` - `DestroyModelMixin`

## Mixins

## Contrete View Classes

## endpoint와의 연결
