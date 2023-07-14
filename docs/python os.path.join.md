---
aliases: 
tags: 
description:
created: 2023-06-21T11:35:55
updated: 2023-07-11T15:21:07
title: python os.path.join
---
[실 사용 예시 {bookstore}](https://github.com/ESTsoft-Book-Project/bookstore/pull/44/files#diff-4392745abac424baa573e9c60413308af753eb912be194dfc224f1e71e81ef98R133)
[os.path.join 사용법{한글 블로그}](https://engineer-mole.tistory.com/188)
[docs.python.org](https://docs.python.org/3/library/os.path.html)

`{BASE_DIR}/media` 디렉토리를 지정하기 위해선 어떻게 해야할까?

`os.path.join(BASE_DIR, 'media')` 하면 된다. 하드코딩하는 것과는 전혀 다른 개념인가보다. 아, 플랫폼 별로 path를 명시하는게 달라서 저렇게 쓰는거구나!

# 자주 쓰는 예시

- 실행파일의 절대경로를 알아내기
```python
os.path.join(os.path.abspath(os.path.dirname(__file__)), "file.py")
```

- 현재 디렉토리 (CWD) 알아내기
```python
os.getcwd(), "file.py"
```

- 리스트를 경로로 만들기
```python
os.path.join(*["C:\\", "Users", "chltm", "workspace", "bookstore"])
```