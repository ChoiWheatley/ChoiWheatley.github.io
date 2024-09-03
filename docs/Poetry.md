---
aliases: 
tags: 
description:
title: Poetry
created: 2024-09-03T11:08:43
updated: 2024-09-03T14:05:33
---

## poetry를 사용하여 가상환경 설정

```jsx
## 폴더 생성
> mkdir oz-backend-django

## 폴더 이동
> cd oz-backend-django

## poetry 초기화
> poetry init

## poetry 의존성 설치 및 가상환경 세팅
> poetry install

## 가상환경 접속하기
> poetry shell
```

## poetry 설치 시 `.venv` 폴더가 프로젝트 내에 생성이 되지 않으시나요?

```python
poetry config virtualenvs.in-project true
poetry config virtualenvs.path "./.venv"
```
