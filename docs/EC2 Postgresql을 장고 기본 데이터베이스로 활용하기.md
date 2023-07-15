---
aliases: 
tags: 
description:
created: 2023-06-27T09:16:47
updated: 2023-07-15T21:33:05
title: EC2 Postgresql을 장고 기본 데이터베이스로 활용하기
---
[[주니어 백엔드 개발자, 그 이상으로 🚀{book-project}|book-project]] 진행하면서 거의 마지막 페이즈에 도달하였다. 물론 치명적인 버그가 발생한 상태라 그걸 먼저 해결해야 하지만, 결국 오눌 16시부터는 postgresql + S3까지 나갈 것이기 때문에 미리 준비를 해놓아야 한다.

참고한 블로그: https://velog.io/@iankimdev/EC2

# ec2

- ubuntu를 EC2에 띄우는 건 생략함
- build-essential, postgresql을 설치한다.
- 서버 설정파일 postgresql.conf을 수정한다.
	- `listen_address = '*'` 원래 로컬호스트였을 거다. 바깥으로 DB에 대한 접근을 열어주자.
- 클라이언트 인증 설정파일인 pg_hba.conf를 수정한다.
	- 어차피 우리가 유저를 제한할 게 아니라서, 모든 IP로 들어오는 모든 유저가 적당한 비밀번호 암호화 알고리즘을 사용하기만 하면 모든 데이터베이스에 대한 접근을 허용하도록 만들어주자.

	```
	# TYPE DATABASE USER ADDRESS    METHOD
	# IPv4 local connections:
	  host all      all  0.0.0.0/0  md5
	# IPv6 local connections:
	  host all      all  ::/0       md5
	```

- postgresql 계정 추가
- pgAdmin 설치 후 서버 등록.
	- Host name/address, Port, username, password 입력
	- 근데 이거 너무 별로야, DBeaver 깔아서 연동하는게 더 나을 것 같다.
- 

# django

장고에서 기본으로 사용하던 sqlite를 버리고 ec2의 postgresql을 사용하는 코드를 작성해보자. 

- 의존성 추가
	- `psycopg2`: postgresql 사용하기 위한 모듈 | [psycopg.org](https://www.psycopg.org/docs/install.html)

	  > For production use you are advised to use the source distribution.
		- [Build prerequisites](https://www.psycopg.org/docs/install.html)에 따르면 `libpq`라는 C 라이브러리에 의존한다. 따라서 `libpq-dev`라는 패키지를 `apt`로 설치하여야 한다.

	- `dj-database-url`: settings.py에서 사용하는 `DATABASES` 변수를 설정해주는 녀석.
		- `DATABASE_URL`로부터 자동으로 세팅을 진행해준다.
- core/db.py
	- decouple_config로 `.env` 파일 안에 들어있는 `DATABASE_URL`을 사용하는 파일이다. `DATABASE_URL`은 아래와 같은 형식을 갖는다. [url schema](https://pypi.org/project/dj-database-url/#url-schema)

	```
	postgres://USER:PASSWORD@HOST:PORT/DBNAME	
	```

- 된건가? 아니 그런데 자꾸 `libpq.so.5` 공유라이브러리가 없다는데?
	- [v] @김영민 그러면 `psycopg2`를 삭제하고 `psycopg2-binary`만 사용해보세요  ↦ 이게 왜 됨? psycopg2-binary는 올바르게 path가 들어오는데 psycopg2는 그렇지 않은 문제인 것 같다.
	- 문제의 원인은 가상환경을 바탕으로 한 linuxbrew python이라서 그런 것 같다. linuxbrew에는 libpq가 설치가 되어있지 않았던 것이다.
	- 하지만 그래도 psycopg2-binary는 꼭 필요한 것 같아보인다.
