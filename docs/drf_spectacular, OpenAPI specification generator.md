---
aliases: 
tags: 
description:
title: drf_spectacular, OpenAPI specification generator
created: 2024-11-12T23:48:45
updated: 2024-11-13T00:09:44
---
- ref
	- <https://drf-spectacular.readthedocs.io/en/latest/readme.html>
	- <https://www.django-rest-framework.org/api-guide/serializers>
---

## Overview

DRF 에서 View 클래스와 Serializer를 만들고 urls.py에 등록하면 자동으로 swagger 또는 redoc 문서를 만들어주는 패키지이다. 

## Story

> '이런 것도 되나?' 싶은 기능들을 미리 작성해두는 곳입니다. 

- 하나의 Serializer에 many 옵션을 지명할 수 있을까? [[extend_schema {drf_spectacular}#request, response, parameter에 Serializer 클래스 또는 Serializer 인스턴스가 들어갈 수 있다.]]
- Authorization을 스웨거 상에서 수행할 수 있다. AuthenticationClass 정의부에 따라갈 것인가, 아니면 별도로 지정해 주어야 하는 걸까?

## Topics

- [[extend_schema {drf_spectacular}]]
