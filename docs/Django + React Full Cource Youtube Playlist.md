---
aliases: 
tags: 
description:
title: Django + React Full Cource Youtube Playlist
created: 2023-07-28T14:30:22
updated: 2023-07-28T14:30:30
---


<iframe src="https://www.youtube.com/embed/soxd_xdHR0o?list=PLOLrQ9Pn6caw0PjVwymNc64NkUNbZlhFw" height="113" width="200" allowfullscreen="" allow="fullscreen" style="aspect-ratio: 1.76991 / 1; width: 100%; height: 100%;"></iframe>

### 알게된 개쩌는 꿀팁들

- [admin site에 등록된 모델들의 컬럼을 정의할 수 있다](https://youtu.be/soxd_xdHR0o?t=3835)

```python
@admin.register(models.Post)
class AuthorAdmin(admin.ModelAdmin):
	list_display = ("title", "id", "status", "slug", "author")
	prepopulated_fields = { "slug": ("title",), }

admin.site.register(models.Category)
```

- [[`objects` member is Manager compositted in Model]]
- [[APIView {drf}]]
