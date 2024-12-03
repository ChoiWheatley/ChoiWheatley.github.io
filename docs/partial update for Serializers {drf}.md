---
aliases: 
tags: 
description:
title: partial update for Serializers {drf}
created: 2024-12-03T21:28:13
updated: 2024-12-03T21:29:19
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
