---
aliases: 
tags: 
description:
title: page-merge-par debugging {pintos}{swjungle}
created: 2023-10-20T15:56:25
updated: 2023-10-20T19:26:20
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

부모와 자식 프로세스 모두 파일을 잘 닫아줬는데, wait한 이후에 파일을 다시 열지 못한다. 다음 디버그 메시지는 `open` 시스템 콜에서 추가했다. 파일 이름 멤버변수도 `file` 구조체에 추가하였다.

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

## page-merge 테스트 관련 질의응답

- 학생: 안녕하세요 조교님 질문이 있습니다  
    tests/vm/에서 page-merge 테스트들을 하다가 문제가 생겼습니다  
    syscall create로 파일을 만들고 성공하는 것까지 확인했습니다. 그 후에 fork로 자식 프로세스를 만들고 자식 프로세스가 부모 프로세스가 만든 파일을 open을 시도할 때 종종 실패할 때가 있습니다.  
    추적하다보면 file_open에서 읽어온 inode가 NULL이었습니다  
    계속 원인을 찾고 있는데 찾지 못해서 질문드립니다  
    혹시라도 상황 해결을 위해 의심해볼만한 것들이 있을까요?
    
- 조교: 안녕하세요 조교 XXX입니다.  
    아직 lab3를 하고 계신다면 filesystem 부분을 건드리지 않으셨을 테고, 그렇다면 file_open은 filesys_open으로부터 호출되었을 것입니다. 이때 넘겨주는 inode 가 NULL이라는 말씀이신가요? 그렇다고 한다면, 에러의 이유는 열려는 파일이 존재하지 않는다는 상황이 가장 유력하고, 그 원인으로는 부모 프로세스에서 만든 파일의 이름이 이상한 경우가 떠오르네요.  
    또한, lab3까지는 파일시스템에 접근할 때는 **항상** lock으로 보호해야 합니다. 혹시 다른 부분에서 이 과정이 빠졌는지 체크해보는 것도 좋을 것 같습니다.  
    그래서 정리하자면,
    
    - 부모 process에서 만든 파일의 이름과 자식 프로세스가 열려는 파일의 이름이 제대로 matching 되는지 printf로 확인해보기
    - 파일 접근하는 모든 함수 (vm 부분에서도 파일 접근합니다)를 lock으로 보호했는지 확인
    
    정도의 추가 가이드를 드릴 수 있을 것 같습니다. 낮은 확률로 메모리 부족이 원인일 수 있는데, 원인이 너무 무궁무진해서 이정도의 답변이 최선인 것 같습니다.  
    도움이 되셨기를 바라며, 추가 질문 있으시면 편하게 해주세요.
