---
aliases: 
tags: 
description:
title: Poetry
created: 2024-09-03T11:08:43
updated: 2024-09-03T14:07:02
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

## CheetSheets

### Poetry 설치

```bash
curl -sSL https://install.python-poetry.org | python3 -
```

또는 Homebrew를 사용:

```bash
brew install poetry
```

### 새로운 프로젝트 생성

```bash
poetry new my-project
```

- `my-project`: 새로 생성할 프로젝트 디렉토리 이름

### 기존 프로젝트 초기화

```bash
poetry init
```

- 프로젝트에서 사용할 패키지 및 설정을 대화형으로 입력

### 가상 환경 활성화

```bash
poetry shell
```

- Poetry가 관리하는 가상 환경을 활성화

### 의존성 추가

```bash
poetry add <패키지명>
```

- 예: `poetry add requests`
- 특정 버전 설치: `poetry add requests@^2.25.1`

### 개발 의존성 추가

```bash
poetry add --dev <패키지명>
```

- 예: `poetry add --dev pytest`

### 패키지 제거

```bash
poetry remove <패키지명>
```

- 예: `poetry remove requests`

### 의존성 설치

```bash
poetry install
```

- `pyproject.toml`에 정의된 모든 의존성을 설치

### 의존성 업데이트

```bash
poetry update
```

- 모든 패키지를 최신 버전으로 업데이트

### 스크립트 실행

```bash
poetry run <명령어>
```

- 예: `poetry run python script.py`

### 패키지 버전 고정

```bash
poetry lock
```

- 현재 설정된 의존성 버전을 `poetry.lock` 파일에 고정

### 프로젝트 테스트

```bash
poetry run pytest
```

- pytest를 사용해 테스트 실행 (pytest는 미리 설치되어 있어야 함)

### 배포용 패키지 빌드

```bash
poetry build
```

- 배포 가능한 소스 배포본과 wheel 패키지를 생성

### PyPI에 배포

```bash
poetry publish
```

- PyPI에 패키지를 배포 (`~/.pypirc` 파일 설정 필요)

### 의존성 목록 보기

```bash
poetry show --tree
```

- 프로젝트의 모든 의존성을 트리 구조로 출력

### 가상 환경 관리

```bash
poetry env list
```

- Poetry가 관리하는 가상 환경 목록 보기

```bash
poetry env use <python 경로>
```

- 특정 Python 버전을 사용하도록 가상 환경 설정

### pyproject.toml 예시

```toml
[tool.poetry]
name = "my-project"
version = "0.1.0"
description = "A sample Python project"
authors = ["Your Name <you@example.com>"]

[tool.poetry.dependencies]
python = "^3.9"
requests = "^2.25.1"

[tool.poetry.dev-dependencies]
pytest = "^6.2.3"

[build-system]
requires = ["poetry-core>=1.0.0"]
build-backend = "poetry.core.masonry.api"
```

## poetry 설치 시 `.venv` 폴더가 프로젝트 내에 생성이 되지 않으시나요?

```python
poetry config virtualenvs.in-project true
poetry config virtualenvs.path "./.venv"
```
