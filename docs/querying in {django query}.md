---
aliases: 쿼리, 질의, queries, relationships
tags: 
description:
created: 2023-06-20T10:48:13
updated: 2024-11-23T20:37:05
title: querying in {django query}
---
- [doc](https://docs.djangoproject.com/en/4.2/topics/db/queries/)

# Before you start (Blog, Author, Entry models)

앞으로 모든 예시는 아래의 코드에서 정의한 모델을 기반으로 제공된다.

```python
from datetime import date

from django.db import models

class Blog(models.Model):
    name = models.CharField(max_length=100)
    tagline = models.TextField()

    def __str__(self):
        return self.name

class Author(models.Model):
    name = models.CharField(max_length=200)
    email = models.EmailField()

    def __str__(self):
        return self.name

class Entry(models.Model):
    blog = models.ForeignKey(Blog, on_delete=models.CASCADE)
    headline = models.CharField(max_length=255)
    body_text = models.TextField()
    pub_date = models.DateField()
    mod_date = models.DateField(default=date.today)
    authors = models.ManyToManyField(Author)
    number_of_comments = models.IntegerField(default=0)
    number_of_pingbacks = models.IntegerField(default=0)
    rating = models.IntegerField(default=5)

    def __str__(self):
        return self.headline
```

- `save` 를 해줘야 데이터베이스에 변경내용이 저장된다. 만약 새로운 필드를 생성하자마자 DB에 넣고싶다면 `create`를 사용할 것.
- `ForeignKey` 필드의 관계는 1:1 관계이므로 단순히 `.`으로 연결하면 된다.
	- `get` 메서드는 오직 하나의 개체만 가져온다는 것을 보장할 때 사용이 가능하다. | 없을 때 → `DoesNotExist` | 두개 이상일 때 → `MultipleObjectsReturned` 그래서 하나의 instance 만 가져오려면 queryset.get() 이 더 유리하다

	 ```python
	e = Entry.objects.get(pk=1)
	b = Blog.objects.get(name="Choi")
	e.blog = b
	```

- `ManyToMany` 필드의 관계는 기본적으로 여러개의 연결이 포함되어 있기 때문에  `filter`나 `all` 따위를 써서  `QuerySet`형태로 얻어와야 한다. | [`objects`](https://docs.djangoproject.com/en/4.2/ref/models/class/#django.db.models.Model.objects)는 모델에 클래스 멤버 형태로 연결돼 있는 `Manager` 인스턴스를 가져온다.
- **all** `Entry.objects.all()`
- **filter** `Entry.objects.filter(pub_date__year=2006)`  | `Entry.objects.filter(headline__startswith="What")`
	- JOIN 연산을 장고 단에서 알아서 수행해 준다. 관계를 확장하려면 원하는 필드에 도달할 때까지 연관 필드의 이름을 `__` 로 구분하여 연결하면 된다. [[querying in {django query}#Field lookups]]
- `filter`의 결과는 `QuerySet`이기 때문에 체이닝이 가능하다. 
- **annotate** 칼럼의 이름을 질의할 때 변경할 수도 있다고 한다. 마치 SQL `AS`와 같다. 사용법은 쿼리시 `annotate` 인자의 kwargs 자리에 원하는 이름을 key값으로 넣으면 그만이라고...

![[스크린샷 2023-07-06 09.32.11.png]]

# Field lookups {filter, get, exclude}

[docs](https://docs.djangoproject.com/en/4.2/ref/models/querysets/#id4)

SQL의 WHERE 절로 변환이 되는 장고 모델 API이다. `QuerySet`로부터  `filter`, `get`, `exclude` 메서드의 키워드 인자로 들어가는 규칙에 대하여 정의한다. 

```
<field>__<lookuptype>=<value>
```

여기에 예외가 존재하는데, `ForeignKey` 필드의 뒤에는 반드시 `_id`가 붙는다는 것이다. (에반데)

`lookuptype`의 종류는 [field lookup reference](https://docs.djangoproject.com/en/4.2/ref/models/querysets/#field-lookups)에서 확인이 가능하다. 일단 자주 쓰이는 타입에 대해서만 알아보자.

- `exact`: exact match
	- `Entry.objects.get(headline__exact="Cat bites dog")`
	- 사실 `__exact` 없이 작성해도 장고는 기본적으로 `__exact`를 붙여준다.
- `iexact`: case-insensitive match
- `contains`: case-sensitive containment test
- `pk`: 사실 `id__exact`의 준말이었다! 연관 테이블의 pk도 검색이 가능하다.
- span relationships
	- 뿐만 아니라, lookup 자체를 연관 테이블을 JOIN한 칼럼까지 지원한다. 물론 JOIN 연산은 우리가 할 필요 없다.
	- `Entry`의 연관 테이블인 `Blog`와 조인하여 블로그의 `name` 필드에 접근할 수 있다.

	 ```python
	Entry.objects.filter(blog__name="Beatles Blog")
	```

	- 조인의 순서를 거꾸로 해도 된다. 이때 규칙은 클래스명을 소문자로 작성하면 된다.

	```python
	Blog.objects.filter(entry__authors__name="Choi")
	```

# Q 객체는 하나의 Lookup 안에 여러 조건을 넣어줄 수 있다.

[[F as field, Q as query in {django query}]]

# EOF
