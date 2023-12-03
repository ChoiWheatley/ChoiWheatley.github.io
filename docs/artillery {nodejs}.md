---
aliases: 
tags: 
description:
title: artillery {nodejs}
created: 2023-12-03T15:16:02
updated: 2023-12-04T00:57:17
---
- [[week14-18 {swjungle}{my own weapon}{nestjs, socketio}]]
- <https://www.artillery.io/docs>
___

## README

본 문서는 NodeJS 부하분산 테스트 프레임워크 artillery를 사용하여 현재 swjungle에서 진행중인 레크리에이션 서비스가 동시에 최대 100명의 유저가 원활하게 **socket.io** 통신을 수행할 수 있는지를 검증하도록 만드는 과정을 작성한 페이지입니다.

Artillery를 사용하면, 격리된 가상의 유저를 만들어 대상 서버에 부하를 줄 수 있다고 한다.

> No memory, network connections, cookies, or other state is shared between virtual users.

로드테스트는 `rampTo` 옵션을 넣어 점진적으로 증가하거나 급격히 증가하는 로드를 시뮬레이트 할 수 있으며, 우리 케이스는 게임을 시작할때 급격한 트래픽이 발생할 것이기 때문에 spike 테스트 위주로 진행하면 될 것 같다.

## artillery quick

별도의 yml 파일을 작성하지 않고도 기본적인 HTTP 요청을 테스트 해볼 수 있다. 여기에서 다음과 같은 테스트를 수행해봤다.

```
artillery quick \
	--count 20 \
	--num 100 \
	https://www.choiwheatley.store
```

다음 명령어는 우리 시연용 페이지의 루트 `/` 엔드포인트에 20명의 가상유저가 각각 100번의 요청을 처리하는 루틴을 수행한다.

테스트 결과는 다음과 같이 나온다. summary report가 10초에 하나씩 출력된다.

```
--------------------------------
Summary report @ 16:34:24(+0900)
--------------------------------

http.codes.200: ............................... 5000
http.downloaded_bytes: ........................ 0
http.request_rate: ............................ 81/sec
http.requests: ................................ 5000
http.response_time:
  min: ........................................ 33
  max: ........................................ 654
  mean: ....................................... 358.1
  median: ..................................... 376.2
  p95: ........................................ 528.6
  p99: ........................................ 572.6
http.responses: ............................... 5000
vusers.completed: ............................. 50
vusers.created: ............................... 50
vusers.created_by_name.0: ..................... 50
vusers.failed: ................................ 0
vusers.session_length:
  min: ........................................ 64326.4
  max: ........................................ 64921.8
  mean: ....................................... 64581.5
  median: ..................................... 64236
  p95: ........................................ 64236
  p99: ........................................ 65533.7
```

[report](https://www.artillery.io/docs/reference/cli/report) 페이지도 생성할 수 있다. `artillery run -o report.json`을 통해 테스트 결과를 JSON으로 받아본 뒤에 `artillery report report.json`을 사용하여 HTML 파일을 받아볼 수 있다.

## Engines :: Socket.IO

<https://www.artillery.io/docs/reference/engines/socketio>

이제 본론으로 들어가보자.

제일 먼저, 열려있는 캐치마인드 방에 100명이 들어가는 코드부터 작성해보자. 가짜 uuid와 가짜 닉네임을 만드는 데 적격인 [falso](https://ngneat.github.io/falso/docs/getting-started) 라이브러리를 사용할 수 있다니 대단하군

**안된다.**

아래 코드는 내가 직접 작성한 열린 캐치마인드 방에 `ready` 요청을 날리는 테스트를 수행한다.

```yml
config:
  plugins:
    fake-data: {}
  target: "https://api.choiwheatley.store"
  phases:
    - name: "join room"
      duration: 10
      arrivalRate: 1
  socketio:
    transports: ["websocket"]
    query: "uuId={{ $randUuid() }}"
scenarios:
  - name: random player joins
    engine: socketio
    flow:
      - namespace: "/catch"
        emit:
          channel: "ready"
          data: 
            room_id: 8
            nickname: {{ $randFullName() }}
```

이게 보니까, falso의 모든 함수를 지원하는 것이 아니었다... `scenarios.flow.log`를 사용하며 삽질을 할 수 있었는데, 그 와중에 하나 건진것이 `randomString(10)`이었다. 이제 `ready` 요청을 보낼때 랜덤한 값의 페이로드를 보낼 수 있게 되었다.

## multiple target

산 넘어 산이다. <https://github.com/artilleryio/artillery/discussions/1471> 대화에 따르면 **Multiple `config` or `scenarios` sections in on YAML file are not supported**라고 못을 박아놨다. 따라서, 100명의 유저가 100개의 uuId를 query param에 담아서 보내는 건 어쩌면 불가능 할지도 모르겠다.
