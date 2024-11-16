---
aliases: 
tags: 
description:
title: Reverse Relation {drf}
created: 2024-11-16T09:37:23
updated: 2024-11-16T10:02:21
---
- ref: <https://www.django-rest-framework.org/api-guide/relations/#reverse-relations>
---
Django와 Django REST Framework(DRF)에서 역방향 관계를 사용하는 방법에 대해 설명하겠습니다. 역방향 관계는 특정 모델이 다른 모델과의 관계를 통해 데이터를 가져오는 방식입니다. Django ORM과 DRF의 `ModelSerializer`에서 역방향 관계를 사용하는 방법을 실제 코드 예제와 함께 정리해 보겠습니다.

### 1. Django에서 역방향 관계 사용하기

Django에서는 `ForeignKey` 필드로 정의된 관계에서 "역방향"으로 데이터를 조회할 수 있습니다. 기본적으로 Django는 `ForeignKey`가 설정된 모델에서 역방향 조회를 위한 매니저(Manager)를 자동으로 생성합니다. 이 매니저의 기본 이름은 `모델명_set` 형식으로 지정되며, `related_name` 옵션을 설정하면 다른 이름으로 지정할 수 있습니다.

예제 코드:

```python
# models.py

from django.db import models

class Blog(models.Model):
    title = models.CharField(max_length=255)

class Entry(models.Model):
    blog = models.ForeignKey(Blog, on_delete=models.CASCADE, related_name='entries')
    headline = models.CharField(max_length=255)
    content = models.TextField()
```

위 코드에서 `Entry` 모델은 `Blog` 모델과 외래 키 관계로 연결되어 있습니다. 이때 `related_name='entries'`를 지정하였으므로, `Blog` 인스턴스에서 관련된 `Entry` 인스턴스를 `entries`라는 이름으로 조회할 수 있습니다.

```python
# 역방향 조회 예제

# 특정 블로그의 모든 Entry를 가져오기
blog = Blog.objects.get(id=1)
entries = blog.entries.all()  # 모든 Entry 객체 조회
print(entries)

# 필터링 예제
filtered_entries = blog.entries.filter(headline__contains="Lennon")
print(filtered_entries)

# 개수 세기
entry_count = blog.entries.count()
print(entry_count)
```

### 2. DRF에서 역방향 관계 직렬화하기

DRF에서 역방향 관계를 직렬화할 때는 `ModelSerializer`의 `fields` 목록에 역방향 관계 필드를 포함해야 합니다. `related_name`이 지정된 경우 이를 이용해 필드를 추가할 수 있으며, 지정하지 않은 경우 기본 이름인 `모델명_set`을 사용해야 합니다.

#### 예제 코드

```python
# serializers.py

from rest_framework import serializers
from .models import Blog, Entry

class EntrySerializer(serializers.ModelSerializer):
    class Meta:
        model = Entry
        fields = ['id', 'headline', 'content']

class BlogSerializer(serializers.ModelSerializer):
    # 역방향 관계 필드 추가 (entries)
    entries = EntrySerializer(many=True, read_only=True)

    class Meta:
        model = Blog
        fields = ['id', 'title', 'entries']
```

`BlogSerializer`에 `entries` 필드를 추가하여, 해당 블로그와 관련된 모든 `Entry` 객체를 역방향으로 가져올 수 있습니다. `many=True` 옵션은 `entries` 필드가 여러 개의 `Entry` 객체를 포함하는 리스트임을 나타내며, `read_only=True`로 설정하여 역방향 관계 필드가 쓰기 가능한 상태가 되지 않도록 설정했습니다.

#### API 응답 예시

블로그 객체를 조회할 때 역방향 관계인 `entries` 필드도 함께 직렬화되어 응답에 포함됩니다.

```json
{
    "id": 1,
    "title": "My Blog",
    "entries": [
        {
            "id": 1,
            "headline": "First Entry",
            "content": "This is the first entry content."
        },
        {
            "id": 2,
            "headline": "Second Entry",
            "content": "This is the second entry content."
        }
    ]
}
```

### 주의 사항

- `related_name`이 설정되지 않은 경우 기본 이름은 `모델명_set` 형식이므로, 예를 들어 `Entry` 모델의 `related_name`이 설정되지 않았다면, `BlogSerializer`에서 `fields=['entry_set']`과 같은 형식으로 사용해야 합니다.
- `HyperlinkedModelSerializer`에서도 비슷하게 사용할 수 있으며, URL 필드를 통해 관련 객체의 URL이 포함됩니다.

이와 같이 Django와 DRF에서 역방향 관계를 설정하고 직렬화하여 API 응답에 포함할 수 있습니다. `related_name`을 사용하여 더 직관적인 필드 이름을 만들 수 있다는 점도 기억해두세요.
