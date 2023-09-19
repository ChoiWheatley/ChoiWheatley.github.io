---
aliases: 
tags: 
description:
title: proxylab
created: 2023-09-17T21:54:51
updated: 2023-09-18T21:30:02
---
- [[week06 {swjungle}{proxy-lab}]]
- [proxylab.pdf](http://csapp.cs.cmu.edu/3e/proxylab.pdf)
- [Tutorial from GFG](https://www.geeksforgeeks.org/creating-a-proxy-webserver-in-python-set-1/)
- [python-proxy-server {GH}](https://github.com/anapeksha/python-proxy-server/blob/main/src/server.py)
- [python mitmproxy practice](https://thepythoncode.com/article/writing-http-proxy-in-python-with-mitmproxy)

## TL;DR

Sequential(40), Concurrency(15), Cache(15)를 채점한다. 만점은 70점이다.

## Assignment Details
### 01. Introduction

웹 프록시는 웹 브라우저와 웹 서버 사이를 중개하는 미들웨어의 역할을 담당한다. 유저가 직접 웹 페이지와 접속하는 대신에, 유저는 프록시에게 요청을 보내게 된다. 프록시는 해당 요청을 받아 웹 서버로 요청을 전달하게 되고, 응답을 받아 다시 클라이언트로 응답을 전달하게 된다.

프록시는 여러 측면에서 유용한데, 먼저 방화벽 뒤에 웹 브라우저가 있을 때에 프록시를 사용하여 방화벽 너머에 있는 웹 페이지에 접속할 수 있다. 그리고 프록시가 사용자 정보와 쿠키를 의도적으로 저장하지 않으면서 익명성을 만족시킬 수도 있다. 프록시는 캐싱도 지원하는데, 한 번 캐싱된 자원에 한하여 굳이 리모트 서버에 요청을 전달할 필요가 없기 때문이다.

이번 과제에서는 세 단계에 걸쳐 프록시 서버를 만들고, 개선해갈 것이다. 먼저 클라이언트의 요청을 읽고 웹 서버로 요청을 전달한다. 웹 서버의 응답을 읽고 클라이언트로 요청을 전달하는 일을 진행한다. 두 번째 단계로는 동시성 프로그래밍을 통하여 다중 커넥션을 다루는 일을 하게 될 것이다. 세 번째 단계는 웹 서버의 응답결과를 캐싱하여 응답속도를 높혀볼 것이다.

## 