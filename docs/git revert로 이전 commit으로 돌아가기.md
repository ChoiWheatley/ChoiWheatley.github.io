---
aliases: 
tags: 
description:
title: git revert로 이전 commit으로 돌아가기
created: 2024-04-11T13:31:04
updated: 2024-04-11T13:33:33
---

## TL;DR

과거의 원하지 않던 커밋을 되돌리고 (UNDO) 그 당시에 취소했던 변경사항을 HEAD에 최신 커밋으로 추가한다.

## Command

```
git revert <Commit>
```

## Caution

log가 생기기 때문에 깔끔한 편이 아님.
