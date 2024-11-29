---
aliases: 
tags: 
description:
title: How can I see the raw SQL queries Django is running
created: 2024-11-30T00:15:34
updated: 2024-11-30T00:18:03
---
- ref: <https://docs.djangoproject.com/en/5.1/faq/models/#faq-see-raw-sql-queries>
---

## TL;DR

```python
>>> from django.db import connection
>>> connection.queries
[{'sql': 'SELECT polls_polls.id, polls_polls.question, polls_polls.pub_date FROM polls_polls',
'time': '0.002'}]

>>> from django.db import reset_queries
>>> reset_queries() # empty connection.queries
```

django shell에서 인터랙티브하게 쿼리를 확인하면서 [[N+1 problem with select_related, prefetch_related {django query}]] 문제도 찾을 수 있을 것이다.
