---
aliases: 
tags: 
description:
title: redis로 access token blacklist 관리하기 {nodejs} {todo}
created: 2023-11-07T08:40:41
updated: 2023-11-07T08:410:359
---
- [[express.js 과제 {swjungle}]]
- [redis 메모리 데이터베이스에 블랙리스트 관리](https://velog.io/@boo105/Redis-%EB%A5%BC-%ED%86%B5%ED%95%9C-JWT-Blacklist-%EA%B5%AC%ED%98%84)
- [npm redis](https://www.npmjs.com/package/redis)

블랙리스트는 만약의 경우에 둘 중의 어느 하나의 토큰이라도 갈취되는 경우 access token의 유효기간이 끝나기 전에 나쁜짓을 하는 것을 차단하기 위한 용도로 사용된다. 로그아웃울 해도 명시적으로 토큰을 비활성화할 수 없다. 서버는 무상태이기 때문이다. 따라서 클라이언트가 스스로 토큰을 비우도록 만들어야 한다. 

redis 메모리 데이터베이스를 활용하면 데이터의 유효기간을 설정해줄 수 있나보다. [expire {redis}](https://redis.io/commands/expire/)
