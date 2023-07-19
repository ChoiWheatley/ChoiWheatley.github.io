---
aliases: 
tags: 
description:
title: django template built in filters 자주 쓰이는 필터 목록
created: 2023-07-19T15:28:39
updated: 2023-07-19T15:58:56
---
[doc](https://docs.djangoproject.com/en/4.2/ref/templates/builtins/#ref-templates-builtins-filters)

## 자주 쓸만한 것들

- `date`: php의 date와 비슷하다고 하다. 문제는 나는 php를 모른다는거. 어쨌든 `{{ value|date:"D d M Y" }}` 이런거 보면 날짜형식의 value를 주어진 형식으로 내보내겠다는 뜻인 것으로 보임.
- `default`: value가 `False`로 평가될 시에 (Falsely) 대치되는 저시기. `{{value|default:"nothing"}}`
- `filesizeformat`: human readable file size format
- `join`: 리스트를 하나의 문자열로 바꾸고, delimeter를 `join` 뒤에 오는 문자열로 대치.
- `length`: 리스트의 길이
