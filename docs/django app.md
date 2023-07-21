---
aliases: 
tags: 
description:
title: django app
created: 2023-07-21T23:41:58
updated: 2023-07-21T23:48:26
---
[Applications {docs}](https://docs.djangoproject.com/en/4.2/ref/applications/)

project ⊃ app 관계에 놓여있음. Project는 장고 웹앱 자체를 의미하며 설정파일, URL 라우팅, 게이트웨이 설정 등을 다룬다. Application은 Project가 의존하는 패키지이며, 여러 프로젝트에서 재사용될 수 있다. 

재사용 되는 application들은 pip 패키지 관리자로 관리가 될 수 있으며 `django-rest-framework`, `tinymce`, `allauth` 등이 있다.
