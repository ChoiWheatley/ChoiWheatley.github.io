---
aliases: 
tags: 
description:
title: HTTPS EC2 {devops}
created: 2023-11-17T14:45:08
updated: 2024-08-21T10:49:19
---
- [[week14-18 {swjungle}{my own weapon}{nestjs, socketio}]]
- [참고 블로그](https://velog.io/@server30sopt/EC2-HTTPS%EB%A1%9C-%EC%97%B0%EA%B2%B0%ED%95%98%EA%B8%B0) - 가비아 호스트를 Route 53과 연결
- [참고 블로그](https://it-eldorado.tistory.com/117) - Route53이 제공해주는 호스트를 결제하여 진행
___

## TL;DR

[[Drawing 2023-11-17 20.33.47.excalidraw]]  
![[Pasted image 20231118205222.png]]

## Flow

1. 클라이언트는 <https://choiwheatley.store> 를 접속한다. DNS 미스가 발생하면 Route53은 4군데의 네임 서버 (NS)에 등록해놓은 캐노니컬 네임 (CNAME) 테이블을 찾아 IP주소를 획득한다. 캐노니컬 네임은 예를 들어 `www.naver.com`이 있으면 `naver.com`이 된다. `www`는 서브도메인.
2. 클라이언트는 해당 IP주소를 향해 요청을 보낸다. 사실 그 IP는 실제 서버의 주소가 아닌 로드 밸런서의 주소이다. 로드 밸런서는 정해진 규칙에 따라 listeners ⟶ target groups 으로 요청을 전가한다. 위의 예시에선 HTTPS와 HTTP 요청을 받아 HTTP 프로토콜의 3000번 포트로 바꿔 EC2 서버에게 요청을 전가한다.
3. 사실 2번과정의 HTTPS 요청에는 한 단계가 더 끼어있다. SSL 인증서를 발급받아야 통신이 가능하기 때문이다. 따라서 사전에 Amazon Certificate Manager로부터 발급받은 certificate를 HTTPS Listener에게 등록하는 것으로 목적을 달성할 수 있다.

## Certbot + Let's Encrypt를 사용하여 SSL PEM 키 발급받기
