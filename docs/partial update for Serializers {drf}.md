---
aliases: 
tags: 
description:
title: partial update for Serializers {drf}
created: 2024-12-03T21:28:13
updated: 2024-12-03T22:49:17
---
- ref: <https://www.django-rest-framework.org/api-guide/serializers/#partial-updates>
---
serializer를 사용하여 업데이트를 사용하기 위해선 Serializer 클래스의 두 키워드 인자를 모두 활용하면 된다:

```python
target_serializer = TargetSerializer(
	instance=instance, 
	data=source_serializer.validated_data, 
	partial=True
)
target_serializer.is_valid(raise_exception=True)
updated_instance = target_serializer.save()
```

Serializer 클래스에 `update` 메서드가 정의되어있어야 한다. `ModelSerializer` 를 상속받은 경우에는 `create`와 `update` 메서드가 자동으로 상속받게 되지만 오버라이드 하여 커스텀 로직을 추가하거나 Partial 옵션에 대응하는 경우를 고려할 수 있다.

```python
def update(self, instance, validated_data):
    for attr, value in validated_data.items():
        setattr(instance, attr, value)
    instance.save()
    return instance
```
