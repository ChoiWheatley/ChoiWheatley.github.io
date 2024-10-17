---
aliases: 
tags: 
description:
title: 10개월이 지난 recre 재정비하기
created: 2024-10-17T23:03:56
updated: 2024-10-17T23:19:00
---
parent: [[week14-18 {swjungle}{my own weapon}{nestjs, socketio}]]

---

## README

treepark.shop 이 만료가 되고 EC2에서 과금이 되기 시작하면서 현재 RecRe서비스는 동작하지 않는 상황입니다. 8월에 한 번 온라인으로 모여 하나의 AWS 계정으로 이주 및 IAM 권한부여를 했으며, 하나의 EC2에 Nest, Next 둘 다 pm2로 실행시켜놓고 로그인, QR 과 같이 당장 안되는 부분을 우선적으로 고쳤습니다. 현재 (2024-10-17) 기준으로 <http://recre.store:3000> 에 들어가지고 로그인 (리디렉션이 안되기는 하지만) 및 게임 플레이가 진행이 되는 것을 확인했습니다.

![[Screenshot From 2024-10-17 23-11-54.png]]

## TODO

> 필수

- SSL 인증서 받아 HTTPS, 443포트로 통신하기
- 로그인 관련 문제 해결하기 
	- 자동으로 홈으로 돌아오지 않음
	- 로그아웃 후에도 로그인 상태가 유지됨 Local Storage가 삭제가 안됨.
	- naver, google 로그인 ❌

> 제안

- Docker와 docker-compose로 컨테이너화하여 관리하기
