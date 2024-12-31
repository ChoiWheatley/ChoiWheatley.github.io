---
aliases: 
tags: 
description: 
links:
  - https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all
status: 
title: Promise.all
created: 2024-12-31T23:04:49
updated: 2024-12-31T23:05:03
---

# Promise.all: 병렬 비동기 작업의 강력한 도구

JavaScript에서 비동기 작업을 처리할 때, 여러 작업의 결과를 기다리거나 특정 작업이 실패했을 때의 동작을 관리하는 것은 중요한 과제입니다. 이를 위해 **Promise.all**은 매우 유용한 도구로 자리 잡았습니다. 이번 글에서는 Promise.all의 동작 원리와 특징, 그리고 사용할 때 주의해야 할 점에 대해 알아보겠습니다.

## Promise.all의 개념

**Promise.all**은 주어진 **Promise 객체의 배열**이나 **iterable 객체**를 입력받아, **모든 Promise가 resolve**될 때까지 기다린 후, 각 결과를 배열로 반환하는 Promise를 생성합니다. 만약 하나의 Promise라도 reject된다면, Promise.all은 즉시 reject 상태가 됩니다.

### 주요 특징

1. **결과 순서 보장**
    
    - 입력 배열의 순서를 유지합니다. 즉, Promise가 resolve되는 순서와 관계없이, 결과 배열의 각 위치는 입력 배열의 Promise 위치와 동일합니다.
2. **전체 성공 조건**
    
    - 모든 Promise가 성공적으로 resolve되어야 최종적으로 resolve됩니다. 실패한 Promise가 있다면 Promise.all은 reject 상태가 됩니다.
3. **단일 실패 처리**
    
    - 하나의 Promise라도 reject되면, Promise.all은 즉시 reject되며, 첫 번째 reject된 Promise의 에러를 반환합니다.
4. **병렬 실행 보장 아님**
    
    - Promise.all 자체는 Promise의 병렬 실행을 보장하지 않습니다. Promise 객체의 실행은 이미 Promise가 생성될 때 시작되며, Promise.all은 단지 모든 Promise가 완료되기를 기다릴 뿐입니다.

## 사용 예제

### 기본 예제: 모든 작업 완료 기다리기

```javascript
const promise1 = Promise.resolve(3);
const promise2 = new Promise((resolve) => setTimeout(resolve, 100, 'foo'));
const promise3 = 42; // Synchronous value

Promise.all([promise1, promise2, promise3])
  .then((results) => {
    console.log(results); // [3, 'foo', 42]
  })
  .catch((error) => {
    console.error(error);
  });
```

위 예제에서:

- `promise1`은 즉시 resolve되는 Promise입니다.
- `promise2`는 100ms 후에 resolve됩니다.
- `promise3`은 동기 값으로 처리됩니다.

Promise.all은 모든 작업이 완료되면 `[3, 'foo', 42]`를 반환합니다.

### 실패 처리

```javascript
const promise1 = Promise.resolve(3);
const promise2 = new Promise((_, reject) => setTimeout(reject, 100, 'error'));
const promise3 = 42;

Promise.all([promise1, promise2, promise3])
  .then((results) => {
    console.log(results);
  })
  .catch((error) => {
    console.error(error); // 'error'
  });
```

`promise2`가 reject되었기 때문에, Promise.all은 전체 작업을 중단하고 에러를 반환합니다.

## 활용 시 주의 사항

1. **독립적인 작업에 사용하기**
    
    - Promise.all은 각 작업이 독립적으로 실행되는 경우에 가장 유용합니다. 만약 작업 간 의존성이 있다면 다른 방법을 고려해야 합니다.
2. **실패 방어**
    
    - 모든 작업이 실패하지 않도록 개별 Promise에 대한 에러 처리가 필요할 수 있습니다. 예를 들어:

```javascript
const promises = [
  fetch('https://api.example.com/endpoint1').catch((error) => error),
  fetch('https://api.example.com/endpoint2').catch((error) => error),
];

Promise.all(promises)
  .then((results) => {
    console.log(results); // 성공한 Promise와 실패한 Promise의 결과를 모두 확인 가능
  });
```

1. **메모리 관리**
    - Promise.all은 입력 배열에 포함된 모든 Promise를 메모리에 유지하므로, 작업 수가 많을 경우 메모리 부담이 발생할 수 있습니다. 작업을 묶어 처리하거나 순차적으로 실행하는 방법을 고려해야 합니다.

## 결론

**Promise.all**은 여러 비동기 작업을 효율적으로 관리할 수 있는 강력한 도구입니다. 병렬로 실행될 수 있는 작업에서 특히 유용하며, 모든 작업의 결과를 한 번에 처리할 수 있습니다. 다만, 하나의 실패가 전체 작업에 영향을 미치므로 이를 방어하기 위한 추가적인 고려가 필요합니다.

Promise.all을 올바르게 활용하여 더 나은 비동기 프로그래밍을 경험해 보세요!

## 연습문제

