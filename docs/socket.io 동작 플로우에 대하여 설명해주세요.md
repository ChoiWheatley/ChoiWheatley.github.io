---
aliases: 
tags: 
description:
title: socket.io 동작 플로우에 대하여 설명해주세요
created: 2024-01-22T14:53:07
updated: 2024-01-22T15:32:10
---
- [[0012 Career 💼]]
- [[socket.io]]
---
Socket.io 통신은 웹 소켓 방식과 HTTP Long Polling 방식을 사용하는데, 그 중 저희팀이 사용한 웹 소켓 방식을 설명드리겠습니다.

클라이언트가 서버에게 HTTP GET 메서드로 요청을 날립니다. 이때 헤더에 Upgrade: websocket 필드가 포합됩니다. 서버는 이에 대한 응답으로 101 Switching Protocols 상태코드를 전송하며, 이때부터 클라이언트와 서버는 순서 상관없이 웹 소켓 프로토콜을 사용하여 통신할 수 있게 됩니다.

## Web Socket이 HTTP와는 다르게 연결을 유지할 수 있는 이유가 무엇인가요?

HTTP, Web Socket 모두 TCP 레이어 위에 있는 어플리케이션 레이어로, HTTP는 RESTful API의 특징인 무상태성을 지원하기 위해 지속적인 연결상태를 보장하지 않는것 뿐입니다. 반면에 Web Socket은 TCP 단에서 체결된 소켓을 기반으로 커넥션을 유지합니다.
