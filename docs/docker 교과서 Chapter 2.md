---
aliases: 
tags: 
description: 도커의 기본적인 사용법
title: docker 교과서 Chapter 2
created: 2024-08-31T21:57:36
updated: 2024-10-07T15:18:06
---

## 요약

도커는 운영체제 수준의 가상화를 제공한다. 가상화된 리눅스 커널 위에서 애플리케이션을 실행시킬 수 있으며, 이는 호스트 기기의 운영체제와는 상관없이 동일하다. 

도커 컨테이너는 애플리케이션과 그것을 실행하기 위해 필요한 최소한의 장치 (파일 시스템, ip주소, 호스트명)만을 사용해 독립적인 가상 리소스가 된다.

도커 엔진은 도커 API를 받아서 도커 컨테이너를 관리하는 컴포넌트로, REST API를 통하여 외부 호스트에서 명령을 전달할 수 있다. 도커 CLI는 도커 API 클라이언트이다. 

## 명령어들

### 컨테이너 실행

```
docker container run diamol/ch02-hello-diamol
```

`diamol/ch02-hello-diamol` 이라는 패키지를 이미지로 실행시킨다. 처음에는 이 이미지가 존재하지 않으므로 도커허브에서 찾아 다운로드 받아 실행시킨다. 이 패키지는 애플리케이션이 필요한 내용만 출력시키고 바로 종료하기 때문에 이미지가 계속 실행되지 않는다.

```
docker container run --interactive --tty diamol/base
```

`--interactive` (줄여서 `-i`) 플래그를 사용하면 컨테이너에 접속된 상태가 된다. `--tty` (줄여서 `-t`) 플래그를 사용하면 터미널 세션을 사용하여 컨테이너에 접속한다.

```
docker container run --detach --publish 8080:80 diamol/ch02-hello-diamol-web
```

`--detach` (줄여서 `-d`) 는 컨테이너를 백그라운드에서 실행시킨다.

`--publish`는 컨테이너의 포트를 호스트 컴퓨터에 공개하는 옵션이다. 인자로 같이 들어오는 `8080:80`의 의미는 호스트 컴퓨터로 8080 포트로 들어오는 요청을 컨테이너 내부 80번 포트로 포워딩 하겠다는 의미이다.

### 컨테이너 보기

```
docker container ls
```

현재 실행중인 컨테이너를 리스트로 출력한다. `-a` 플래그를 붙이면 종료한 컨테이너까지 출력한다. 아래 예시처럼 나온다.

```
CONTAINER ID   IMAGE                          COMMAND   CREATED         STATUS    PORTS     NAMES
6c0fff1a446e   diamol/ch02-hello-diamol-web   "-d"      5 minutes ago   Created   80/tcp    vigilant_bouman
```

```
docker container top <CONTAINER_ID>
```

컨테이너에서 실행중인 프로세스의 정보를 출력한다.

```
docker container logs <CONTAINER_ID>
```

컨테이너가 STDOUT으로 출력한 모든 문자를 출력한다.

### 컨테이너 삭제

```
docker container rm -f $(docker container ls -a --quiet)
```

`--quiet` 명령어는 `CONTAINER_ID` 만 출력한다. 따라서 위 명령어는 모든 도커 컨테이너를 제거하는 명령어이다.

```
docker container rmi $(docker images --quiet)
```

도커 컨테이너를 지웠다고 이미지가 같이 지워지는 것은 아니다. 실행중인 프로세스를 종료했다고 실행파일이 삭제되지 않듯이 컨테이너를 지운다고 이미지가 같이 지워지지 않는다.

### 컨테이너 원격 명령 (exec)

```
Usage:  docker container exec [OPTIONS] CONTAINER COMMAND [ARG...]

Execute a command in a running container

Aliases:
  docker container exec, docker exec

Options:
  -d, --detach               Detached mode: run command in the background
      --detach-keys string   Override the key sequence for detaching a container
  -e, --env list             Set environment variables
      --env-file list        Read in a file of environment variables
  -i, --interactive          Keep STDIN open even if not attached
      --privileged           Give extended privileges to the command
  -t, --tty                  Allocate a pseudo-TTY
  -u, --user string          Username or UID (format: "<name|uid>[:<group|gid>]")
  -w, --workdir string       Working directory inside the container
```

아하, 이래서 도커 컨테이너에 셸로 들어가는 명령어는 `docker container exec -it CONTAINER sh` 이렇게 COMMAND 칸에 셸 프로그램의 이름을 적으면 되는 거였구나!!

## Docker API

docker 엔진을 사용하기 위해선 (상태를 보고 컨테이너를 생성하는 등의 모든 조작을 의미) 도커 API를 통해야 한다. HTTP REST API를 통하여 인터페이스가 열려있다고 보면 되고, 우리가 지금껏 사용했던 docker-cli는 도커 API의 클라이언트 중에 하나인 것이다. 따라서 원한다면 GUI 프로그램을 설치하여 도커의 상태를 확인할 수도 있을 뿐더러, 외부 환경이나 클라우드에서 돌고 있는 도커를 원격으로 작업할 수도 있다.

> [!warning] 그러면 내가 직접 도커 관리 시스템을 구축할 수도 있다는 소리네? [[Docker API를 활용하여 도커 관리 시스템 구축]]
