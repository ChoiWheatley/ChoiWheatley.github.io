---
aliases: 
tags: 
description:
title: page-merge-par debugging {pintos}{swjungle}
created: 2023-10-20T15:56:25
updated: 2023-10-20T17:38:24
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

부모와 자식 프로세스 모두 파일을 잘 닫아줬는데, wait한 이후에 파일을 다시 열지 못한다.

```
(page-merge-par) begin
(page-merge-par) init
(page-merge-par) sort chunk 0
[*] 📴 close "buf0" (page-merge-par)
(page-merge-par) sort chunk 1
[*] 📴 close "buf0" (child-sort)
child-sort: exit(123)
[*] 📴 close "buf1" (page-merge-par)
(page-merge-par) sort chunk 2
[*] 📴 close "buf1" (child-sort)
child-sort: exit(123)
[*] 📴 close "buf2" (page-merge-par)
(page-merge-par) sort chunk 3
[*] 📴 close "buf2" (child-sort)
child-sort: exit(123)
[*] 📴 close "buf3" (page-merge-par)
(page-merge-par) sort chunk 4
[*] 📴 close "buf3" (child-sort)
child-sort: exit(123)
[*] 📴 close "buf4" (page-merge-par)
(page-merge-par) sort chunk 5
[*] 📴 close "buf4" (child-sort)
child-sort: exit(123)
[*] 📴 close "buf5" (page-merge-par)
(page-merge-par) sort chunk 6
[*] 📴 close "buf5" (child-sort)
child-sort: exit(123)
[*] 📴 close "buf6" (page-merge-par)
(page-merge-par) sort chunk 7
[*] 📴 close "buf6" (child-sort)
child-sort: exit(123)
[*] 📴 close "buf7" (page-merge-par)
(page-merge-par) wait for child 0
(page-merge-par) open "buf0": FAILED
page-merge-par: exit(1)
```
