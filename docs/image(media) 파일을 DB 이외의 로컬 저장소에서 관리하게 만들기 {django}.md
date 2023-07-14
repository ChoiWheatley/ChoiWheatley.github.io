---
aliases: 
tags: 
description:
created: 2023-06-21T11:25:46
updated: 2023-07-11T15:21:08
title: image(media) 파일을 DB 이외의 로컬 저장소에서 관리하게 만들기 {django}
---
- [Products image #44](https://github.com/ESTsoft-Book-Project/bookstore/pull/44)
- 프론트
	- JSON Base64 형식으로 이미지 전송
	- [x] detail_product 템플릿에 이미지 표시
- 백
	- Media URL
		- 미디어만을 다루는 폴더 세팅 및 static url 추가

- dependencies
	- [pillow](https://pypi.org/project/Pillow/) 이미지 프로세싱 라이브러리

- [[python os.path.join]]

# FileField, ImageField
[Managing files {docs.djangoproject.com}](https://docs.djangoproject.com/en/4.2/topics/files/)

로컬 저장소를 활용하기 위해 장고는 기본적으로 `MEDIA_ROOT`, `MEDIA_URL` 세팅을 사용한다. 저장소에 대한 선택권은 우리에게 있다. [[File storage {django}]]를 확인해보라.

DB에는 파일이 들어가지 않는다. `FileField`나 `ImageField`의 경우 Path와 URL만이 저장된다.
- path
	- `MEDIA_ROOT` : 파일을 관리하는 로컬 시스템의 절대 경로
	- `MEDIA_URL` : `MEDIA_ROOT`로부터 파생된 서브디렉토리 경로를 의미하며, 상대경로이다. 파일 요청이 들어오면 해당 경로를 따라 접근하게 된다.
- url
	- 업로드된 파일의 URL을 의미하며, 값을 그대로 웹 브라우저에 넣어 GET 요청을 보내면 해당 파일을 다운받을 수 있다.

파일명이나 파일 위치를 바꾸는 행위, 파일을 수정하는 행위는 반드시 해당 필드의 `save` 메서드 호출을 필요로 한다.

# Static files
[[How to manage static files (e.g. images, js, css) {django}]]로 가세요

# FileReader js object
[[FileReader {js}]]

# File Storage
[File storate {docs.djangoproject.com}](https://docs.djangoproject.com/en/4.2/topics/files/#file-storage)

- [ ] TODO: 읽어볼 것

# Go Further
[[Resource Storage와 Database를 연동하는 방법에 대하여 알아봅니다.]]