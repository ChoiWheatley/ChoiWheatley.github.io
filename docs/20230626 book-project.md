---
aliases: 
tags: 
description:
created: 2023-06-26T15:15:00
updated: 2023-07-11T15:21:10
title: 20230626 book-project
---
- [[get_object_or_404로 쿼리를 수행한다 {django}]]
- comment 버튼을 다른 유저의 댓글도 삭제할 수 있다.
- 로그인 안 된 상태에서 cart로 들어가기 방지
- 73번 이슈: 회원가입을 양식에 맞지 않게 한 경우, 회원가입이 정상적으로 진행 되었다가 로그인 시에 Undefined 에러를 출력.
- profile 페이지에서 아무것도 수정하지 않은 채 수정하기 버튼을 눌러도 PATCH 요청이 날아간다.
- signout 

# 오늘은

EC2 instance 생성, EC2 내 Postgresql 설치, DB관련 코드 작성

- @김영민 의 라이브 코딩쇼를 보고 나중에 각자 EC2 구축할거임. TODO: 관련 문서 요청할 것.
- `/etc/postgresql/14/main/postgresql.conf` 파일을 열어  `listen_address`를 전부 (`*`)로 변경한다.
- `/etc/postgresql/14/main/pg_hba.conf`는 아무래도 인바운드 규칙 설정하는듯.
- vscode를 열어 core/db.py를 만들어서 database 기본 설정 및 연결에 대한 정보를 끌어온다. (`import dj_database_url`)
	- `.env` 파일에서 `DATABASE_URL` 부분을 `postgresql://<username>:<password>@<ec2-ip-addr>:5432/main` 으로 변경.
	- settings.py에서 db 연결해주고


# 책을 써야지

[[Securities about {https} and {jwt {cookie}, {session}}]]