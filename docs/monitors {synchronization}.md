---
aliases: 
tags:
  - scrap
description: 
title: monitors {synchronization}
created: 2023-09-25T19:55:45
updated: 2023-09-25T20:26:09
---
- <<https://en.wikipedia.org/wiki/Monitor_(synchronization>)>  
- [[synchronization {pintos} {semaphore} {lock} {monitor}]]에서 조사하다가 발견한 문서
- <https://casys-kaist.github.io/pintos-kaist/appendix/synchronization.html#monitor-example>
___
모니터는 다양한 조건을 감지하는 스레드를 위해서 사용할 수 있다. condition variable과 lock을 함께 쓴다. 모니터에 접근하기 위해서 lock이 사용되고, 한 번에 하나의 스레드만 모니터에 들어갈 수 있다. 모니터 안에서 데이터베이스 트랜잭션 커밋이라던가 유저 입력, 타이머 같은 어떤 조건이 만족될 때까지 condition variable이 제공하는 waiters 컨테이너에 들어가 대기상태가 된다.

![[Pasted image 20230925194140.png]]

대표적인 모니터 활용방법으로는 Producer & Consumer 패턴이 있다. [pintos-kaist](https://casys-kaist.github.io/pintos-kaist/appendix/synchronization.html#monitor-example)에 좋은 예제가 있으니 확인바란다.

간략하게 정리하자면 읽기만 하는 스레드, 쓰기만 하는 스레드 각각 모니터에 들어가 주어진 조건이 만족할 때에만 본연의 작업을 수행한다. 예를 들어 Producer는 `put` 함수를 사용해 상호배제적으로 모니터에 들어간 뒤 문자를 넣을 수 있는 조건을 가진 `not_full` condition variable 컨테이너에 들어가 대기한다. 반대편(Consumer)에서 문자를 읽은 뒤 시그널을 보내면 대기명단 중 첫번째 스레드가 깨어나 조건이 일치하는 경우 버퍼에 문자를 넣는다. Producer가 모니터를 나가기 전에, 적어도 하나의 문자가 추가됐으므로 Consumer들에게 이를 알리기 위해 `not_empty` condition variable에 시그널을 보낸다. 
