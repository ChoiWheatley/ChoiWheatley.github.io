---
aliases: 
tags: 
description:
title: Decorator - {nestjs}
created: 2023-12-01T02:17:57
updated: 2023-12-01T12:52:28
---
- [[0018.1 Nest.js 🪺]]
- [공식문서 링크](https://docs.nestjs.com/custom-decorators)
___

## README

[[week14-18 {swjungle}{my own weapon}{nestjs, socketio}]]에서 진행하고 있는 NestJS 프로젝트 진행도중 데코레이터를 사용하면 좋을 것 같은 부분이 많이 보여 직접 만들어 보면서 공부하려고 한다.

## DIY :: Query

`client.handshake.query[param?: string]` ⟶ `@WSQuery(param?: string)` : 어차피 똑같이 HTTP/S로 돌아가는 거니까 써도 되지 않을까? 했지만 직접 찍어보니 null이 나왔다.

```ts
@SubscribeMessage('stop')
async stop(client: Socket, payload: { cur_time: Date }, @Query('uuId') uuid: string) {
	Logger.debug('uuid: ' + uuid);
```

```
[Nest] 6106  - 12/01/2023, 11:40:52 AM   DEBUG uuid: null
```

그래서 별도의 `@Wsquery('uuId')` 데코레이터를 만들었고 위에서 처럼 사용하였고, 이번엔 uuId가 잘 찍혀져 나왔다. 하지만 payload를 분해할 수 없다는 에러를 맞이하고 말았다. 페이로드는 인자의 순서에 영향을 받는지라 만약 이렇게 쓰고 싶다면 payload를 위한 데코레이터를 하나 더 만들어야 할 것이다. 일단 나는 빠르게 가드를 만들어봐야 하기 때문에 이부분은 넘기도록 하겠다.

## DIY :: Guard

이번에 만들어볼 것은 game의 status를 검사하는 로직을 가드에 담아볼 것이다. user uuid 혹은 host uuid에 따라 game 인스턴스를 불러오는 방식이 다르고, 가드를 걸 상태가 총 세가지 (`wait`, `playing`, `end`)나 있기 때문에 아마도 Reflection을 사용하여야 할 것 같아보인다.

<https://docs.nestjs.com/fundamentals/execution-context#reflection-and-metadata>

새로운 `Reflector#createDecorator`를 사용할 수도 있고, 가드를 걸고싶은 컨텍스트에`@SetMetadata()` 데코레이터를 직접 사용해도 된다. 
