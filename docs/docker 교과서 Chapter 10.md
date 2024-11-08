---
aliases: 
tags: 
description:
title: docker 교과서 Chapter 10
created: 2024-11-08T22:56:52
updated: 2024-11-08T23:33:01
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

## 도커 컴포즈 환경변수와 비밀값 (environment, env_file, secrets)

환경별로 설정값을 주입하기에 매우 유용하다. 오버라이드를 할때 도커 엔진 또는 파일시스템에 저장된 비밀 키값을 `source`에 넣고 컨테이너에 저장될 위치를 `target`에 명시하면 된다.

```yml
services:
  todo-web:
	secrets:
	  - source: todo-db-connection
	    target: /app/config/secrets.json
```
