---
aliases: 
tags: 
description:
title: Use docker in WSL2 distro
created: 2023-11-02T00:21:13
updated: 2023-11-02T00:22:08
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
