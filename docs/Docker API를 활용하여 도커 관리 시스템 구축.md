---
aliases: 
tags: 
description:
title: Docker API를 활용하여 도커 관리 시스템 구축
created: 2024-10-07T15:08:33
updated: 2024-10-07T15:11:37
---
- parent: [[0010 Programming 👩‍💻|programming]] 
- [docker docs](https://docs.docker.com/reference/api/engine/version/v1.39/)


[[docker 교과서 Chapter 2#Docker API]] 에서 발췌함:

> docker 엔진을 사용하기 위해선 (상태를 보고 컨테이너를 생성하는 등의 모든 조작을 의미) 도커 API를 통해야 한다. HTTP REST API를 통하여 인터페이스가 열려있다고 보면 되고, 우리가 지금껏 사용했던 docker-cli는 도커 API의 클라이언트 중에 하나인 것이다. 따라서 원한다면 GUI 프로그램을 설치하여 도커의 상태를 확인할 수도 있을 뿐더러, 외부 환경이나 클라우드에서 돌고 있는 도커를 원격으로 작업할 수도 있다.

> [!warning] 그러면 내가 직접 도커 관리 시스템을 구축할 수도 있다는 소리네?
