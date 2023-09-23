---
aliases: 
tags: 
description:
title: 2023-09-23 dump {swjungle}
created: 2023-09-23T18:55:55
updated: 2023-09-23T20:06:00
---

## appendix thread

- `thread_init` 새 스레드 공간 할당하는데 `thread_start` 뭐하는 놈임?
	- 부트스트랩에 의해 처음 main함수에 rip가 올라왔을때
- main에서 idle 스레드는 ready queue에 다른 스레드가 기다리고 있는지 확인하지 않고 바로 생성하는데 왜?
	- 스케줄러를 실행하기 위해서 idle 스레드를 만드는 것이다. 타이머 인터럽트 끝나고 나서 실행이 되는게 스케줄러인데, 

## priority scheduling

- H가 L을 기다리는 상태 (blocking), M은 ready queue에 있는 상태, L은 H가 기다리는 lock을 가지고 있는 상태일때, L이 running으로 올라오자마자 바로 yield를 한다. 
- 나보다 높은 우선순위에 있는 스레드가 ready queue에 있다는 것은 어떻게 알지? yield를 하기 전에 자기가 직접 호출해야 하는건가?
- 우선순위 max를 갱신하면서 ready queue에서 꺼낼때 자기가 yield 할 지 안할지 판단하는 의견 나옴. 그러면 max는 언제 줄어드냐?

## Alarm Clock

- 낮은 우선순위의 waiting 스레드 but 높은 우선순위의 blocking 스레드는 기부 당해서 running 상태로 먼저 올라가지 않을까?  

![[Pasted image 20230923200404.png]]

- busy waiting 방식을 사용하지 않게 타이머를 만들어라.
