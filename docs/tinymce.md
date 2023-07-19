---
description:
aliases: 
tags: 
created: 2023-05-29T17:54:31
updated: 2023-07-20T06:47:11
title: tinymce
---
[official link](https://www.tiny.cloud/tinymce/)  
[django-tinymce doc](https://django-tinymce.readthedocs.io/en/latest/)
___
- wysiwyg rich-text embeddable editor
- [tinymce 사용하는 YT Link](https://youtu.be/sMqDJovFO-Y?t=7351)

## how to use it *outside* the admin site?

문서가 오지게 불친절하고, 심지어 쓸데도 없는 `FlatPage`에 의존하는 코드를 넣어놔서 한참을 헤맸다. 그래도 한 마디로 정리하자면,

> [!NOTE]  
> `django-tinymce`모듈은 미리 정의된 Form ==Widget==이다
