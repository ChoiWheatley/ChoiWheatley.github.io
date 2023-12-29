---
aliases: 
tags: 
description:
title: Use docker in WSL2 distro
created: 2023-11-02T00:21:13
updated: 2023-11-14T08:56:28
---
- [[0110 Utility 🔧]]
- [[0010 Programming 👩‍💻|programming]]
___

## REAME

[[express.js]] 공부하다가 mongodb를 설치할 일이 있어 설치하다가 발견한 문제.

<https://www.mongodb.com/docs/manual/tutorial/install-mongodb-community-with-docker/>

설치하려고 봤더니 WSL distro 안에서 `docker` 앱을 사용할 수 없다고 나온다. 분명히 예전에 WSL 안에서 도커 쓴 기억이 있어서 좀 찾아봤더니...

## Solution

<https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-containers>

도커 데스크톱 설정에서 `Settings > Resources > Wsl Integration` 탭으로 가서 원하는 디스트로를 토글하면 도커 명령어를 사용할 수 있게 만들어준다고!

## Plus Alpha: port forwarding

`-p <inbound-port>:<destination-port>`를 `docker run` 명령어 사이에 끼워넣어주면 된다.

```
docker ps
CONTAINER ID   IMAGE      COMMAND                  CREATED         STATUS         PORTS                    NAMES
fd3697bcf65c   postgres   "docker-entrypoint.s…"   2 seconds ago   Up 2 seconds   0.0.0.0:5432->5432/tcp   recre-db
```

## 매번 원격 데스크톱을 켜서 docker desktop을 켜야하는 문제

<https://stackoverflow.com/questions/51252181/how-to-start-docker-daemon-windows-service-at-startup-without-the-need-to-log> 다음 대화에서 Task Scheduler를 사용하여 컴퓨터 부팅 1분 뒤에 자동으로 태스크를 실행하도록 만들 수 있었다.
