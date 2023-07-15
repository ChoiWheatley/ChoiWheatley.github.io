---
description:
aliases: 
tags: 
created: 2023-06-05T10:31:27
updated: 2023-07-15T21:33:03
title: python venv 환경설정
---

# for UNIX-based systems

1. [pyenv](https://stackoverflow.com/a/59268046/6428901)를 사용하여 사용할 파이썬의 버전을 명시할 수 있음.
2. `python<version> -m venv <environment-name>` 예시: `python3.11.3 -m venv venv`
3. `. <environment-name>/bin/activate`

# for Windows systems

1. https://www.python.org 에 접속하여 원하는 파이썬 버전을 설치함.
2. 설치한 파이썬의 절대경로를 복사한다. (윈도우 키 누른 뒤에 `Python 3.11.3` 검색 후 Open File Location)
3. `<python-path>\python.exe -m venv <environment-name>` 예시: `C:\Users\chltm\AppData\Local\Programs\Python\Python311\python.exe -m venv venv`
4. `.\<environement-name>\Scripts\Activate.ps1`

# 공통

- `python -V`를 사용하여 내가 사용하려는 파이썬 버전을 확인가능. 
- [[내가 실행중인 python의 절대경로를 알고싶다면]]
