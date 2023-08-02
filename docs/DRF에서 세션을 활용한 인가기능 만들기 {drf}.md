---
aliases: 
tags: drf/permissions drf/authorization
description:
title: DRF에서 세션을 활용한 인가기능 만들기 {drf}
created: 2023-08-02T09:42:38
updated: 2023-08-02T10:40:42
---
![Permissions and Custom Permissions {YT}](https://youtu.be/5AOn0BmSXyE)

- 인증의 범위로 따진다면 | [permissions {doc}](https://www.django-rest-framework.org/api-guide/permissions/#setting-the-permission-policy)
	- project level permissions
	- object level permissions
	- view level permissions
- 인증의 효력으로 따진다면 | [permissions {doc}](https://www.django-rest-framework.org/api-guide/permissions/)
	- `AllowAny`
	- `IsAuthenticated`
	- `IsAdminUser`
	- `IsAuthenticatedOrReadOnly`
- 인증의 수단으로 따진다면 | [authentication {doc}](https://www.django-rest-framework.org/api-guide/authentication/#api-reference)
	- Token based authentication
	- Session based authentication
