---
description:
aliases: 
tags: 
created: 2023-05-31T11:40:54
updated: 2023-07-18T23:16:56
title: django template
---
- <https://docs.djangoproject.com/en/4.2/ref/templates/language/>
- django template != Jinja2

```html
{% extends "base_generic.html" %}

{% block title %}{{ section.title }}{% endblock %}

{% block content %}
<h1>{{ section.title }}</h1>

{% for story in story_list %}
<h2>
  <a href="{{ story.get_absolute_url }}">
    {{ story.headline|upper }}
  </a>
</h2>
<p>{{ story.tease|truncatewords:"100" }}</p>
{% endfor %}
{% endblock %}
```

# Variables

`{{ variable }}`

- `page_obj`

# Filters

내장 필터들이 있었구만. 자세한 건 그때그때 파헤쳐 보자구.

`{{ name|lower }}`

# Tags

가장 많이 마주친 구문. 태그는 마치 프로그래밍 언어의 키워드처럼 예약된 단어들이 있고, 이것들은 기존의 지식과 꽤나 잘 들어맞는다.

`{% tag %}`

어떤 태그는 시작 태그와 종료 태그가 한 쌍이다.

- `for - endfor` 
- `if - elif - else - endif`
- `extends ` + `block - endblock`  ==> a powerful way of cutting down on "boilerplate" in templates. | [template inheritance](https://docs.djangoproject.com/en/4.2/ref/templates/language/#template-inheritance)
- `url <url> [params]`
- `with - endwith` : 변수를 선언할 수 있다;;;;

# context

[[django template {context}]]로 가세요.  
[[get_context_data, get_queryset { django } { ListView }]]

# Configuration in `settings.py`

<https://docs.djangoproject.com/en/4.2/topics/templates/#configuration>

템플릿 파일을 저장하는 별도의 위치를 정의하고 싶다면 settings.py의 `TEMPLATES` 속성을 건들이자!
