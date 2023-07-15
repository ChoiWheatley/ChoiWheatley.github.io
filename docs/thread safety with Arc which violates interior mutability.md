---
description:
aliases: 
tags: 
created: 2023-04-02T17:29:23
updated: 2023-07-15T21:33:03
title: thread safety with Arc which violates interior mutability
---
- https://rust-unofficial.github.io/too-many-lists/third-arc.html
- 위 사이트는 말 그대로 thread safety에 대한 찍먹만 소개했다. [[Too Many Linked Lists]]에서 persistent list를 구현하는 데 사용한 `Rc` 키워드를 단지 `Arc`로 바꾸기 위해 한 페이지를 할애하다니...
- TL; DR: `Arc`는 원자적인 연산을 사용하여 레퍼런스를 카운팅 하는 thread safe한 포인터이다. 하지만 러스트 개발자들이 정의한 thread saftey 규칙 전부를 만족하지는 못하는데, 그것은 바로 interior mutability, 내부값 가변을 지키지 못했기 때문이다. 
- `Send`와 `Sync` 마크 (메서드가 하나도 없는 트레이트)는 각각 move와 레퍼런스 공유에 대한 범주를 위한 것들이다. 거의 대부분의 타입들은 `Send`이다. 모든 변수들은 값을 소유하기 때문이다. 많은 종류의 타입들은 `Sync`이다. 스레드 간에 데이터를 공유하기 위해선 shared reference에 데이터를 래핑하여야 하는데, 그걸 지원하는 타입들은 전부 `Sync`이다. (아님 말고..)
- interior mutability를 정의하는 두 가지 클래스가 있다. `cell`은 단일 스레드에서만, `lock`은 멀티스레드에서.
- 
