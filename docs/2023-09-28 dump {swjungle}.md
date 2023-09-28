---
aliases: 
tags: 
description:
title: 2023-09-28 dump {swjungle}
created: 2023-09-28T17:07:34
updated: 2023-09-28T17:11:23
---
- [[week07-09 {swjungle} {pintos}]]
___

## PR 15 분석

<https://github.com/ChoiWheatley/swjungle-week07-09/pull/15>

**`thread.c`** 변경사항 분석을 ai한테 맡겼다

- In the `thread_create` function, after unblocking a thread, a check is added to compare the newly created thread's priority with the current executing thread's priority. If the new thread's priority is higher, the current thread yields its execution.

- In the `thread_set_priority` function, after setting the new priority for the current thread, a similar check is added. If the ready list is not empty and the new priority is higher than the priority of the first thread in the list, the thread yields its execution. The commit message was "edit(thread_set_priority, thread_create, do.sh)", which suggests that these changes were made to modify the behavior of thread priority handling in order to ensure proper preemption when higher-priority threads are created or when a thread's priority is changed.
