---
aliases: 
tags: 
description:
created: 2023-07-06T10:02:19
updated: 2023-07-15T21:33:03
title: updating in {django query}
---

## 업데이트할 객체를 변수에 저장하여 각 필드에 접근하는 방법

```python
post = Post.objects.get(pk=pk)
post.title = 'new title'
post.save()
```

##  업데이트 할 객체에 직접 접근하는 방법

```python
Post.objects.get(pk=pk).update(content='new content')
```
