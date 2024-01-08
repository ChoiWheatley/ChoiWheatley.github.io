---
aliases: 
tags: 
description:
title: busy waiting 방식이 무엇인가요
created: 2024-01-08T20:37:57
updated: 2024-01-08T21:00:03
---
- [[0012 Career 💼]]
- [[week07-10 {swjungle} {pintos}]]
---
busy waiting 방식이란 OS에서 원하는 자원을 얻기 위해 기다리는 것이 아니라 **권한을 얻을 때까지 확인**하는 것을 의미합니다.

> busy waiting 방식의 단점과 그 보완책을 설명해주세요

busy waiting 방식은 계속해서 스레드가 ready queue로 이동하기 때문에 불필요한 CPU 자원을 낭비하게 됩니다. 예를 들어 ready queu에 본 스레드 하나밖에 없는 경우, busy wait 방식을 사용하게 되면 yield를 해봤자 스케줄러에 의해 다시 선택이 되고, 다시 yield를 하는 무한루프에 빠지게 됩니다.

따라서 스레드에 block 상태를 추가해 조건을 만족하기 전까지 스레드를 sleep_list에 추가하는 방향으로 구현했습니다. 이 경우, 하드웨어 틱에 의해 실행되는 핸들러에 의해 스레드를 깨우게 됩니다.
