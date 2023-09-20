---
aliases: 
tags: 
description:
title: stack {C}
created: 2023-09-20T14:42:53
updated: 2023-09-20T14:47:05
---

## README

`node_t` 알아서 선언해야 한다.

## stack.h

```c
#pragma once
/**
 * Stack
 * @brief maxlength가 정해진 단순 스택
 */
#include <stdbool.h>

#define MAX_STACK 20000

typedef struct node_stack {
  node_t *arrptr[MAX_STACK];
  int top;
} Stack;

bool stack_full(const Stack *stack);
bool stack_empty(const Stack *stack);
bool stack_push(Stack *stack, node_t *new);
node_t *stack_pop(Stack *stack);
```

## stack.c

```c
#include <stack.h>

bool stack_full(const Stack* stack) { return stack->top >= MAX_STACK; }

bool stack_empty(const Stack* stack) { return stack->top <= 0; }

bool stack_push(Stack* stack, node_t* new) {
  if (stack_full(stack)) {
    return false;
  }
  stack->arrptr[stack->top] = new;
  stack->top += 1;
  return true;
}

node_t* stack_pop(Stack* stack) {
  if (stack_empty(stack)) {
    return NULL;
  }
  stack->top -= 1;
  return stack->arrptr[stack->top];
}
```
