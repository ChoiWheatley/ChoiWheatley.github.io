---
aliases: 
tags: 
description:
title: 지연시간 극복 {swjungle}{recre}
created: 2023-12-04T17:39:07
updated: 2023-12-04T22:13:17
---
- [[week14-18 {swjungle}{my own weapon}{nestjs, socketio}]]
---
- 지연시간을 끊임없이 계산하는 요즘 게임처럼 구현할 정도는 아니지 않을까? 
- start_game호스트 이벤트를 받으면 3초의 시간여유 사이에 한 번만 브로드캐스트를 돌려 모든 응답이 돌아올때까지의 시간을 측정하는 것이다. 
- 지금 이미 서버는 플레이어들에게 `start_game` 브로드캐스트를 보내는데, 여기에 대한 `im_ready` 메시지를 플레이어들이 일괄적으로 보내도록 해보자. `RedGreenGame`엔티티에 `start_at`이라는 속성과 `ping`이라는 속성을 추가, `im_ready` 메시지를 받을때 `ping`을 그때그때 갱신한다.
- [web socket latency 관련 블로그](https://ankitbko.github.io/blog/2022/06/websocket-latency/) 에서는 클라이언트가 스스로 지연시간을 측정한다. 생각해보면 게임을 할때 클라이언트 화면 위에 지연시간이 뜨는게 맞기는 한듯. 

생각해본 api들이다.

```
// server -> player
ping: {
	server_ts: number
}

// player -> server
ping.ack: {
	server_ts: number,
	client_ts: number
}

// server -> player
pong: {
	server_ts: number,
	client_ts: number,
	server_ack_ts: number
}
```

수상할정도로 지연시간이 크게 나온다. 다음은 지연시간 계산결과를 측정한 뒤 각종 통계를 내본 결과다. 통계내는 numpy 코드는 GPT한테 부탁했다.

```
평균: 67.1118869296637ms
최소: 5.526527002512012ms
최대: 348.7045019865036ms
99% 백분위수: 200.6986215996742ms
95% 백분위수: 124.32711260021168ms
```

아래는 EC2에 올려서 IMDB를 사용했을때 결과

```
평균: 59.65141448760648ms
최소: 0.4221919924020767ms
최대: 405.82011999189854ms
99% 백분위수: 179.18522739261363ms
95% 백분위수: 115.07675500215555ms
```
