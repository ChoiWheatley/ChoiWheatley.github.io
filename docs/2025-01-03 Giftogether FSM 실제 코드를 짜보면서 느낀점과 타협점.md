---
aliases: 
tags:
  - giftogether
description: 
links:
  - "[[FSM and FST]]"
status: 
title: 2025-01-03 Giftogether FSM 실제 코드를 짜보면서 느낀점과 타협점
created: 2025-01-03T22:28:30
updated: 2025-01-03T23:36:23
---

# [[FSM and FST]]에서 실제 코드를 짜보면서 느낀점과 타협점

## Event를 커맨드 대용으로 사용

**BEFORE**

```typescript
export type Command = string;

export interface Transition<State> {
  from: State;
  to: State;
  command: Command;
  event: IEvent;
}
```

**AFTER**

```typescript
export type EventName = string;

export interface Transition<State> {
  from: State;
  to: State;
  event: EventName;
}
```

1. **실제 요구사항과 설계의 간극**:

	Mealy Machine 방식은 이론적으로 정교하지만, 실제로는 Event만으로 상태 전환을 트리거하고 결과를 나타내는 것이 충분했다. Command를 별도로 관리하는 것은 오히려 과도한 설계가 되었다.

2. **이벤트 중심의 아키텍처**:

	이벤트 기반 시스템에서는 Event가 트리거와 상태 전환 결과의 역할을 동시에 맡는 경우가 많다. 이를 활용하면 상태 전환 로직을 간소화할 수 있었다.

3. **간결함과 유지보수성**:

	Command와 Event를 분리하면 새로운 상태 전환을 추가할 때 두 가지를 모두 정의해야 했다. 그러나 이벤트 하나만 정의하도록 변경하면서 코드의 간결함과 유지보수성이 크게 향상되었다.

## Event 클래스 자체를 string key로 활용

FSM 설계에서 `event`의 타입을 `Enum`이 아니라 `string`으로 설정한 이유는 다음과 같은 장점 때문일 거라 생각한다.

### 1. **확장성**

`Enum`은 정적으로 정의된 값의 집합으로 제한되며, 새로운 이벤트를 추가하려면 `Enum` 정의를 수정해야 한다. 반면, `string`은 런타임에서도 유연하게 새로운 이벤트를 처리하거나 추가할 수 있다.

- **예시**: `DepositMatchedEvent.name`을 `Enum`으로 처리했다면 새로운 이벤트를 추가할 때마다 `Enum` 정의를 업데이트해야 했다. 하지만 현재 방식은 이벤트 클래스를 추가하는 것만으로 충분하다.

### 2. **폴리모피즘 활용**

각 이벤트 클래스가 고유한 `name` 속성을 가지게 함으로써, 이벤트 이름을 클래스 생성자에서 자동으로 가져올 수 있다. 이를 통해 `Enum` 없이도 이벤트 클래스의 책임을 명확히 하고 코드 중복을 줄일 수 있다.

- **코드에서 확인**:

    ```typescript
    export abstract class BaseEvent implements IEvent {
      name: EventName = this.constructor.name;
    }
    ```

    이렇게 설계하면 이벤트 이름은 클래스 자체에서 자동으로 생성된다.

### 3. **이벤트 핸들링 단순화**

`string`을 이벤트 식별자로 사용하면 이벤트 핸들링 로직이 더 직관적이고 간결해진다. 특히, 이벤트 이름을 기반으로 동적으로 핸들러를 등록하거나 실행할 때 유용하다.

- **코드에서 확인**:

    ```typescript
    @OnEvent(DepositMatchedEvent.name)
    async handleDepositMatched(event: DepositMatchedEvent) {
      // 처리 로직
    }
    ```

    `@OnEvent` 데코레이터가 문자열 이름을 사용함으로써, 이벤트 이름과 핸들러 간의 연결이 간단해진다.

### 4. **외부 시스템과의 통합**

`string` 타입은 메시지 브로커나 이벤트 버스 같은 외부 시스템과의 통합에서 더 유리하다. 문자열 기반의 이름은 표준 이벤트 구조와 자연스럽게 통합된다.

- **예시**:

    ```typescript
    eventEmitter.emit(event.name, event);
    ```

    여기서 `event.name`이 문자열로 되어 있어 메시지 버스와의 상호작용이 용이하다.

### 5. **타입 안정성 유지**

`BaseEvent`를 사용해 모든 이벤트가 동일한 구조를 따르도록 설계했기 때문에, `Enum` 대신 `string`을 사용해도 타입 안정성을 잃지 않는다. 이는 추상 클래스(`BaseEvent`)와 인터페이스(`IEvent`)로 보장되고 있다.

### 6. **런타임에서의 유연성**

`Enum`은 정적으로 정의되지만, `string`은 런타임에서도 동적으로 정의할 수 있다. 이는 플러그인 형태의 동적 로직이나 외부 이벤트를 처리해야 하는 시스템에서 매우 유용하다.

---

### 요약

`event`를 `string`으로 정의한 이유는 확장성과 유연성, 폴리모피즘 활용, 외부 시스템과의 통합 용이성 때문이다. 또한, 타입 안정성을 유지하면서도 런타임에서 동적 처리가 가능하도록 설계한 점이 이 선택의 주된 이유라 볼 수 있다.
