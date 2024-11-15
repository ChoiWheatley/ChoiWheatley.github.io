---
aliases: 
tags: 
description:
title: docker 교과서 Chapter 10
created: 2024-11-08T22:56:52
updated: 2024-11-15T17:59:38
---
도커 컴포즈는 어떤 상황에서 많이 쓴다고? ⇒ 여러 컨테이너를 하나의 도커 엔진에서 돌려야 하는 경우, 특히 작은 규모의 서비스를 퍼블리시 할때, 아니면 테스트, 개발환경에서 자주 쓰인다. 이번 장에는 도커 컴포즈의 고급 기능을 활용하여 **환경**에 따라 애플리케이션의 동작을 다르게 하는 케이스에 대응하고자 한다.

-  어떤 환경이 있는가?
	- 개발
	- 배포
	- 테스트
		- 유저
		- 운영체제
		- 핫픽스
		- ...

도커 컴포즈로 실행한 컨테이너의 이름을 자세히 보면 크게 두 부분으로 나뉘는 것을 볼 수 있다. 컨테이너 이름의 접두어로 **프로젝트명**이 붙고 그 뒤에 애플리케이션 이름이 붙는 방식이다. 이때 프로젝트 명이란, 실행한 도커 컴포즈 파일이 위치한 폴더이름을 기준으로 할 수 있으며 `-p` 옵션을 통해 임의로 정의할 수도 있다. 이름만 다르면 서로 다른 컨테이너이기 때문에 같은 컨테이너를 프로젝트 이름만 다르게 설정하여 여러번 up 할 수 있다.

```sh
$ docker-compose -f ./todo-list/docker-compose.yml -p todo-test up -d
$ docker-compose -f ./todo-list/docker-compose.yml -p todo-hotfix up -d
$ docker-compose -f ./todo-list/docker-compose.yml -p todo-os up -d
```

문제는 도커 컴포즈 안에서 이미 포트번호를 정의해놓은 경우, 두번째 up부터는 랜덤으로 정해진 포트를 사용하게 된다. 이 경우 `docker port <container-name> <inner-port>`를 사용해야 하는데, 매번 그렇게 할 수는 없잖아?

## 도커 컴포즈 오버라이드

기본 문법:

```sh
$ docker-compose -f <base-compose> -f <override-compose> [-f <override-compose> ...] up
$ docker-compose -f <base-compose> -f <override-compose> [-f <override-compose> ...] config # ⇒ 이 옵션은 up 하기 전에 오버라이드 결과를 출력하는 유용한 명령이다.
```

이때 오버라이드 하려는 컴포즈 파일은 베이스 컴포즈 파일과 정확히 같은 구조를 가진 상태에서 옵션을 바꾸어야 한다. 예를 들어 `services.todo-web.image`의 값을 바꾸고 싶으면 이 구조를 그대로 가져가고 변경할 값을 지정해주어야 한다는 것이다. 포트 구성을 바꾸면 좋겠는걸

*docker-compose.yml*

```yml
services:
  todo-web:
    image: diamol/ch06-todo-list
    ports:
      - 80
    environment:
      - Database:Provider=Sqlite
    networks:
      - app-net

networks:
  app-net:
```

*docker-compose-v2.yml*

```yml
services:
  todo-web:
	secrets:
	  - source: todo-db-connection
	    target: /app/config/secrets.json
```

**OVERRRRRRRRRRRRRRRRRRRRRRRRRRRRRRIDE**

```sh
$  docker-compose -f docker-compose.yml -f docker-compose-v2.yml config

name: todo-list
services:
  todo-web:
    environment:
      Database:Provider: Sqlite
    image: diamol/ch06-todo-list:v2 // ⇐ 여기가 오버라이드 되었다!!!
    networks:
      app-net: null
    ports:
      - mode: ingress
        target: 80
        protocol: tcp
networks:
  app-net:
    name: todo-list_app-net
```

### 오버라이드 유스케이스

- `docker-compose.yml`: 베이스 컴포즈. 서비스 정의부. 포트나 도커 네트워크는 ❌
- `docker-compose-dev.yml`: 개발 환경 대상 설정. 도커 네트워크 정의. 웹포트 정의, API포트 정의. 헬스체크 ❌ 디펜던시 체크 ❌
- `docker-compose-test.yml`: 테스트 환경 대상 설정. 도커 네트워크 정의, 헬스체크 정의. 웹포트 정의, API 포트 ❌
- `docker-compose-uat.yml`: 사용자 인수 테스트 설정. 도커 네트워크 정의. 웹포트 정의. 헬스체크 주기를 더 자주, unhealty 상태가 되면 재시작 옵션 추가.

## 도커 컴포즈 환경변수와 비밀값 (environment, env_file, secrets)

환경별로 설정값을 주입하기에 매우 유용하다. 오버라이드를 할때 도커 엔진 또는 파일시스템에 저장된 비밀 키값을 `source`에 넣고 컨테이너에 저장될 위치를 `target`에 명시하면 된다.

아래는 `docker secret create todo-db-connection` 을 해야만 동작할 것이다. 그렇지 않다면 아래 아래의 예시와 같이 `secrets` 최상단 프로퍼티를 정의해야 한다.

```yml
services:
  todo-web:
	secrets:
	  - source: todo-db-connection
	    target: /app/config/secrets.json
```

아래는 시크릿 파일을 컴포즈에서 따로 정의해주었다. 

- `environment`: 환경변수 키밸류 쌍을 문자열로 직접 정의할 수 있다.
- `env_file`: 텍스트 파일의 경로를 값으로 받아 환경변수를 자동으로 정의해준다. 셸 환경변수 선언과 같은 문법을 가진다.
- `secrets`: **최상단 프로퍼티**임에 주의. networks처럼 요소의 실제 시크릿 값 또는 값이 저장돼 있는 경로가 정의된다.

```yml
services:
  todo-web:
    ports:
      -  8089:80
    environment:
      -  Database:Provider=Sqlite
    env_file:
      - ./config/logging.debug.env

secrets:
  todo-db-connection:
    file: ./config/empty.json
```

### 로컬 환경변수 이용하기

오버라이드가 싫다면 로컬 시스템의 환경변수를 가져다 쓰는 방법이 있다. 문법은 `${VAR}`로, 유닉스 변수 사용과 동일하다. 한 번에 하나의 환경변수를 `export` 할 것이 아니라, env 파일을 생성하여 `source` 하거나 아예 셸파일을 사용하는 것이 정신건강에 좋을 것 같아보인다.

```yml
todo-web:
  ports:
    - "${TODO_WEB_PORT}:80"
  ...
```

## [[YAML 확장 필드]]

 
YAML 확장 필드와 앵커(`&`)와 별칭(`*`)을 활용해 중복 구성을 줄일 수 있습니다. 예를 들어, 공통 환경을 `x-common-env`에 정의하고 `<<: *common-env`로 참조하거나, 기본 설정을 `&defaults`로 정의한 뒤 필요한 서비스에서 `<<: *defaults`로 적용해 일관성을 유지합니다. 이로써 Docker Compose 등에서 효율적 구성 관리가 가능합니다.

```yaml
default-settings: &defaults
  timeout: 30
  retries: 3

service1:
  <<: *defaults
  timeout: 60  # 이 서비스는 기본 설정을 재정의합니다.

service2:
  <<: *defaults
```

## 도커를 이용한 설정 워크플로 이해하기

이 챕터는 현업에서 대규모 프로젝트를 운영하는 경우에 대한 다양한 유스케이스를 소개한다. 애플리케이션 구성 요소를 유연하게 설정하는 경우, 환경 (포트, 볼륨, 네트워크 등)에 대한 결정을 미루는 경우, 로그수준이나 캐시 크기와 같이 애플리케이션 동작 config를 다르게 가져가야 하는 경우 등이 있다.

## 연습 문제

> todo-list 애플리케이션을 두 가지 환경으로 설정해보자.

1. Dev (기본)
	1. 로컬 파일 DB 사용
	2. 8089번 포트 사용
	3. to-do 애플리케이션의 v2 태그 실행
2. Test
	1. 별도의 DB 컨테이너 사용
	2. DB 스토리지를 위한 볼륨 정의
	3. 8080번 포트 사용
	4. to-do 애플리케이션의 latest 태그 실행 

필요한 파일의 개수는 3개, `docker-compose.yml` ⇒ Base, `docker-compose-dev.yml` ⇒ Dev, `docker-compose-test.yml` ⇒ Test 

맨 땅에 헤딩할 필요는 없다. `/ch10/exercise/todo-list-configured` 실습의 내용을 참고해서 쓰면 된다.

==`docker-compose.yml`==

```yaml
version: "3.7"

services:
  todo-web:
    image: diamol/ch06-todo-list
    secrets:
      - source: todo-db-connection
        target: /app/config/secrets.json
```

==`docker-compose-dev.yml`==

db 설정을 줄때 로컬 파일 데이터베이스 (Sqlite) 쓰는 옵션은 환경변수 `Database:Provider`의 값을 `Sqlite`로 해주는 것이었다.

포트 8089를 오픈하기 위해서는 `ports`를 사용하면 된다.

to-do 이미지 `diamol/ch06-todo-list:v2`를 쓰면 된다.

```yaml
services:
  todo-web:
    image: diamol/ch06-todo-list:v2
    ports:
      - 8089:80
    environment:
      - Database:Provider=Sqlite
```

==`docker-compose-test.yml`==

별도의 DB 컨테이너를 사용하라고 하는데, PostgreSQL을 사용하면 된다. 

- 환경변수는 `Database:Provider=Postgres`
- 서비스 하나를 더 정의해야 한다. `image: diamol/postgres:11.5` (근데 얘는 무슨 포트를 열어야 하나?)
	- 이 서비스의 `PGDATA` 환경변수를 따로 설정해주어야 한다. 이 환경변수는 데이터베이스 서버가 사용할 볼륨의 위치를 정의하기 때문에 볼륨 지정도 따로 해줘야 한다.
		- `volumes: "todo-database:/data"`
		- `PGDATA=/data`
	- 이 서비스는 secret.json 파일을 필요로 한다.
- 8080 포트 사용, 얘는 쉽지 이젠 `ports: 8080:80`
- 이미지의 latest 태그 사용 `diamol/ch06-todo-list:latest`

```yaml
services:
  todo-web:
    ports:
      - "8080:80"
    environment:
      - "Database:Provider=Postgres"
    image: diamol/ch06-todo-list:latest
    networks:
      - app-net

  todo-db:
    image: diamol/postgre:11.5
    environment:
      - "PGDATA=/data"
    volumes:
      - "todo-database:/data"
    ports:
      - "5433:5432"
    networks:
      - app-net
    secrets:
      - source: postgres-connection
        target: /app/config/secrets.json

networks:
  app-net:
    external:
      name: nat

volumes:
  todo-database:

secrets:
  postgres-connection:
    file: secret.json
```
