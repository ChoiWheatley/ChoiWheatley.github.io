---
aliases: 
tags: 
description:
title: queue {C}
created: 2023-09-20T14:44:34
updated: 2023-09-20T14:46:39
---

## README

`node_t` 타입을 알아서 선언하여야 한다. C로 일반화 프로그래밍 할 줄 모름.

## queue.h

```c
#pragma once
/**
 * Queue
 * @brief maxlength가 정해진 단순한 원형 큐
 */
#include <stdbool.h>
#define MAX_QUEUE 10000

typedef struct node_q {
  node_t *arrptr[MAX_QUEUE];
  int head;
  int tail;
} Queue;

bool queue_full(const Queue *queue);
bool queue_empty(const Queue *queue);
void queue_push(Queue *queue, node_t *new);
node_t *queue_pop(Queue *queue);
```

## queue.c

```c
#include <queue.h>

bool queue_full(const Queue *queue) {
  return ((queue->tail + 1) % MAX_QUEUE) == queue->head;
}

bool queue_empty(const Queue *queue) { return queue->tail == queue->head; }

void queue_push(Queue *queue, node_t *new) {
  if (queue_full(queue)) {
    // full, discard head's element
    queue->head = (queue->head + 1) % MAX_QUEUE;
  }
  queue->tail = (queue->tail + 1) % MAX_QUEUE;
  queue->arrptr[queue->tail] = new;
}

node_t *queue_pop(Queue *queue) {
  if (queue->head == queue->tail) {
    // no element
    return NULL;
  }
  queue->head = (queue->head + 1) % MAX_QUEUE;
  return queue->arrptr[queue->head];
}
```
