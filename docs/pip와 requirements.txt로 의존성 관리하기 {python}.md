---
aliases: 
tags: 
description:
created: 2023-07-06T13:42:37
updated: 2023-07-11T15:21:07
title: pip와 requirements.txt로 의존성 관리하기 {python}
---
- `pip freeze > requirements.txt`
- `pip install -r requirements.txt`

## problem: pip package version conflict

requirements.txt파일의 버전을 우선하고 싶다면 `--no-deps` 옵션을 추가하면 된다.