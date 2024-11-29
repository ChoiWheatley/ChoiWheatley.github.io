---
aliases: 
tags: 
description:
created: 2023-07-06T10:22:03
updated: 2024-11-29T22:04:32
title: N+1 problem with select_related, prefetch_related {django query}
---
`JOIN` 고도화에 사용되는 함수들. 단일 쿼리에서 계속 `JOIN`을 수행하면 비효율적이므로 미리 조인을 시켜놓은 결과를 객체로 반환한다.

# `select_related`

JOIN을 하는 타이밍이 DB상에서 이루어진다. 따라서 [[참조와 역참조]] 에서 다룬 JOIN이 이루어지는 위치에 따라 생각해보면... DB에서 JOIN을 하니까 정참조로는 `ForiegnKey`, `OneToOne` 모두 가능했지만, 역참조는 `OneToOne`만이 가능하다. 왜? 아직 파이썬 메모리 안에 대응수에 대한 정보가 없을 때 (N:M, 1:N) 오직 DB 쿼리만으로 역참조에 대한 정보가 어렵기 때문이다.

- 정참조
	- `ForeignKey`
	- `OneToOne`
- 역참조
	- `OneToOne`

```python
books = Book.objects.select_related("author")
for book in books:
	print(book.author.name)
```

Book 안에 author를 ForeignKey로 들고 있는 경우 select_related를 사용하면 JOIN을 한번만 사용하여 books 객체와 각 author를 참조할 수 있다.

# `prefetch_related`

JOIN할 대상을 먼저 가져온 뒤 파이썬 상에서 JOIN이 이루어진다. 따라서 [[참조와 역참조]]에서 다룬 JOIN이 이루어지는 위치에 따라 생각해보면... 이미 테이블이 파이썬 메모리 안에 다 들어와 있기 때문에 역참조가 `select_related`보다 훨씬 유연한 편이다. 

- 정참조
	- `ForeignKey`
	- `ManyToMany`
- 역참조
	- `ForeignKey`
	- `ManyToMany`

```python
authors = Author.objects.prefetch_related("book_set")
for author in authors:
	for book in author.book_set.all():
		print(book.title)
```

# 성능비교

connection.queries를 가지고 쿼리가 몇번이나 호출되었는지를 확인할 수 있다:

**without optimization**

```python
from django.db import connection, reset_queries

# Clear queries log
reset_queries()

books = Book.objects.all()

# Access related objects (may trigger N+1 problem)
for book in books:
    print(book.author.name)

# Print executed SQL queries
for query in connection.queries:
    print(query['sql'])
```

**optimized query**

```python
from django.db import connection, reset_queries

reset_queries()

# Optimized query
books = Book.objects.select_related('author')
for book in books:
    print(book.author.name)

# Check the queries
for query in connection.queries:
    print(query['sql'])
```
