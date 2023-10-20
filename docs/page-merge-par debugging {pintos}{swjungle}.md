---
aliases: 
tags: 
description:
title: page-merge-par debugging {pintos}{swjungle}
created: 2023-10-20T15:56:25
updated: 2023-10-20T16:02:27
---
- [[week09 - Virtual Memory {pintos} {swjungle}]]
___

## `exec` 실패 문제

`process_exec` > `load` > `filesys_open` > `dir_lookup` > `lookup` > `inode_read_at` 에서 `inode.data`의 값이 이상한 값이 들어있어 exec에서 간헐적으로 실패함.

이 문제는 `syn-read`, `syn-write` 버그와 공유되는 것 같음.

파일을 읽는 것 뿐만 아니라 파일을 여는데에도 lock이 필요한가? 그래서 전역적으로 lock을 선언하여 인터페이스로 열고 닫을 수 있게 해봤음.

```c
// threads/thread.c

void load_lock_acquire() {
  lock_acquire(&g_load_lock);
}

void load_lock_release() {
  lock_release(&g_load_lock);
}

```

`load` 호출 앞, 뒤로 lock을 걸었더니 뭔가 조금 느려진 것 같지만 적어도 `exec`에서 터지지는 않는 것 같다.

## 닫은 파일을 다시 열지 못하는 문제

