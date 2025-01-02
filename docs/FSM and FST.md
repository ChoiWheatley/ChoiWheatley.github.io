---
aliases: 
tags: 
description: 
links:
  - https://t-pl.io/ddd-aggregates-processes-state-machines-and-transducers
status: 
title: FSM and FST
created: 2025-01-02T16:31:40
updated: 2025-01-02T16:55:59
---
참고한 블로그: [https://t-pl.io/ddd-aggregates-processes-state-machines-and-transducers](https://t-pl.io/ddd-aggregates-processes-state-machines-and-transducers)

---

## Finite State Machines

유한상태머신 (Finite State Machine)은 프로세스 주도 개발에서 채택되는 상태를 모델링하는 방법이다. 시간에 따른 객체의 상태를 모델링하기 위해 시스템은 아래의 매개변수들을 활용한다.

- Commands
- Behaviors
- Events

시간의 흐름에 따라 시스템에 커맨드(Command)가 전송이 되고, 시스템은 스스로 본인의 상태(Behavior)를 변경한다.

State Machine 세상에서 시스템은 다섯개의 튜플로 정의된다:

- S: finite set of States
- C: finite set of Commands
- δ: set of Behaviors, a relation between a state and command to a state
- S0: the initial state
- F: finite set of final state

$$ A=\left( S,C,\delta:S \times C \rightarrow S, S_0: S_0 \in S, F: F \subset S \right) $$

---

## Finite State Transducers

> Transducer: a device that converts variations in a physical quantity, such as pressure or brightness, into an electrical signal, or vice versa.

유한상태변환기(Finite State Transducer)는 FSM에 이벤트를 더한 것에 불과하다. 그래서 시스템에 커맨드가 전송이 되었을때 시스템은 내부적으로 상태를 바꾸는 것 뿐만 아니라 외부에 이벤트를 발행할 수 있게된다.

- 왜 이벤트를 발행해야 하는건데?
    - 시스템의 상태변화에 대한 정보로 외부로 전달하여 다른 시스템이 이 이벤트에 필요한 작업을 수행할 수 있도록 하는 것이다.
    - 예: 결제 시스템에서 “결제 완료” 이벤트가 발행되면, 이를 구독하는 배송 시스템이 “배송 준비” 프로세스를 시작할 수 있다.

FST 세상에서 시스템은 총 7개의 튜플로 정의된다:

- FSM에서 정의된 다섯개의 튜플: S, C, δ, S0, F
- E: finite set of Events
- ω: the output relation of a state and command pair to an event

$$ A=\left( S,C,E,\delta:S \times C \rightarrow S, \omega: S \times C \rightarrow E, S_0: S_0 \in S, F: F \subset S \right) $$

---

## 수학적 모델을 타입스크립트 코드로 변환

```typescript
// Define the FSM with states, commands, events, and transitions
type State = string;
type Command = string;
type Event = string;
type Output = string;

// Define the structure of a transition
interface Transition {
  from: State; // Starting state
  to: State;   // Destination state
  command: Command; // Triggering command
  output: Output;   // Emitted output (event)
}

// FSM class to manage transitions
class MealyFSM {
  private state: State; // Current state
  private readonly transitions: Transition[]; // Transition rules
  private readonly finalStates: Set<State>; // Set of final states

  constructor(
    private readonly initialState: State,
    transitions: Transition[],
    finalStates: State[]
  ) {
    this.state = initialState;
    this.transitions = transitions;
    this.finalStates = new Set(finalStates);
  }

  // Retrieve the current state
  public getState(): State {
    return this.state;
  }

  // Handle a command, transition state, and emit output
  public dispatch(command: Command): Event {
    const transition = this.transitions.find(
      (t) => t.from === this.state && t.command === command
    );

    if (!transition) {
      throw new Error(
        `Invalid transition: No transition for command "${command}" from state "${this.state}"`
      );
    }

    // Apply transition
    this.state = transition.to;
    console.log(
      `Transitioned to state "${this.state}" using command "${command}". Output: "${transition.output}"`
    );

    return transition.output; // Emit the associated output (event)
  }

  // Check if the current state is a final state
  public isFinalState(): boolean {
    return this.finalStates.has(this.state);
  }
}

// Example usage of FSM

const transitions: Transition[] = [
  { from: "S0", to: "S1", command: "CMD1", output: "EVENT1" },
  { from: "S1", to: "S2", command: "CMD2", output: "EVENT2" },
  { from: "S2", to: "S3", command: "CMD3", output: "EVENT3" },
  { from: "S3", to: "F", command: "CMD4", output: "EVENT4" },
];

const fsm = new MealyFSM("S0", transitions, ["F"]);

// Dispatch commands
console.log("Initial State:", fsm.getState()); // S0
fsm.dispatch("CMD1"); // S1
fsm.dispatch("CMD2"); // S2
fsm.dispatch("CMD3"); // S3
fsm.dispatch("CMD4"); // F

// Check if the final state is reached
if (fsm.isFinalState()) {
  console.log("Reached final state!");
}
```
