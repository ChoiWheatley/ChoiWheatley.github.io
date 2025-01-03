---
aliases: 
tags:
  - giftogether
description: 
links: 
status: 
title: 2025-01-03 Giftogether
created: 2025-01-03T22:28:30
updated: 2025-01-03T22:33:12
---

## [[FSM and FST]]에서 실제 코드를 짜보면서 느낀점과 타협점

**Event를 커맨드 대용으로 사용**

**Event 클래스 자체를 값으로 활용**

내가 FSM Transition을 설계할 때 event 값을 Enum이 아니라 `Class<IEvent>`으로 한 이유

event enum과 event payload를 따로 설계하기가 싫었기 때문. 그래서 클래스 이름을 가져오는 자바스크립트 방법인 `this.constructor.name`을 사용하기로 했고, 이것을 `IEvent`는 `name`이라는 프로퍼티로 정의, `BaseEvent`가 이 프로퍼티를 구현함으로써 아래와 같이 일반 이벤트들은 `BaseEvent`를 확장하기만 하면 되었다:

```typescript
export interface Class<T> {
  new (...args: any[]): T;
}

export type EventType = Class<IEvent>;

export interface Transition<State> {
  from: State;
  to: State;
  event: EventType;
}

export class DepositMatchedEvent extends BaseEvent {
  constructor(
    public readonly deposit: Deposit,
    public readonly provisionalDonation: ProvisionalDonation,
  ) {
    super();
  }
}

@Injectable()
export class DepositFsmService {
  private readonly transitions: Transition<State>[] = [
    {
      from: State.Unmatched,
      to: State.Matched,
      event: DepositMatchedEvent,
    },
    {
      from: State.Unmatched,
      to: State.Orphan,
      event: DepositUnmatchedEvent,
    },
    {
      from: State.Matched,
      to: State.Refunded,
      event: DepositRefundedEvent,
    },
  ];

  transition(current: State, event: EventType): State {
    const transition = this.transitions.find(
      (t) => t.from === current && t.event === event,
    );

    if (!transition) {
      throw new Error(
        `Invalid transition: No transition for event "${event}" from state "${current}"`,
      );
    }

    return transition.to;
  }
}

```
