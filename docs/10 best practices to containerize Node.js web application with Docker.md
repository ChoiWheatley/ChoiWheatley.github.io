---
aliases: 
tags: 
description:
title: 10 best practices to containerize Node.js web application with Docker
created: 2024-10-27T18:46:19
updated: 2024-10-27T18:46:54
---
- ref: <https://snyk.io/blog/10-best-practices-to-containerize-nodejs-web-applications-with-docker/>

---

![[NodeJS-cheat-sheet.webp]]

1. 구체적이고 공식적으로 지원하는 이미지 태그를 사용하자. alpine 태그는 비공식이라 피해야 한다. lts라던가 latest는 제로데이 공격에 취약하다.
2. 프로덕션 의존성만을 추가하자. devDependencies를 제외하고 의존성을 설치하는 명령어는 npm ci --only=production 이다.
3. 환경변수를 프로덕션용으로 설정하고 자동으로 런타임이 최적화를 할 수 있도록 하자. ENV NODE_ENV production
4. 루트권한으로 실행되는 것을 피하자.
5. 도커는 노드 런타임을 PID 1번에 실행한다. 보안상의 위협이 될 수 있으므로 [dumb-init](https://github.com/Yelp/dumb-init) CLI를 사용하자. 서버가 비정상적으로 종료되어도 부모 프로세스가 안전하게 시그널을 받아낼 수 있다.
6. 시그널을 받아 Node.js 애플리케이션이 종료될 때 인스턴스들을 안전하게 해제한 뒤에 프로세스를 종료하자.
7. snyk 광고. 컨테이너가 사용중인 이미지에 취약점이 있는지를 파악해주는 CLI 프로그램이라고.
8. 멀티 스테이지 빌드를 활용하자.
9. .dockerignore를 사용하자. node_module, .env 파일이 이미지로 올라가는 것을 막는다. .env 파일은 컨테이너화할때 환경변수로 넣어야 한다.
10. 정 시크릿 파일을 이미지로 등록시켜야 한다면 mount secret 옵션을 사용하자.
