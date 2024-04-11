---
aliases: 
tags: 
description:
title: git reflog를 사용하여 reset hard 로 사라진 커밋을 복구할 수 있다고
created: 2024-04-11T10:52:05
updated: 2024-04-11T10:58:01
---
reference와 log의 합성어로, HEAD의 참조 이력을 로그 형태로 출력해 주는 명령어이다.

## 명령어

```
git reflog
```

프로젝트가 위치한 커밋이 바뀔 때마다 기록이 되는 내역들을 출력한다. 여기에는 리셋이 된 내역도 포함이 되어있기 때문에 `git reset --hard`로 날아간 최신 커밋들의 해시값도 알아낼 수 있다. 그리고 그 해시값으로 다시 `git reset --hard`하여 복구가 가능하다.

## 실습

A → B → C 커밋 생성 후 A 쪽으로 하드리셋. log와 reflog를 사용하여 출력 차이점 확인. C 커밋의 해시값을 복사하여 재 하드리셋
