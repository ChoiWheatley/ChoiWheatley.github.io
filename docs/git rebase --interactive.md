---
aliases: 
tags: 
description:
title: git rebase --interactive
created: 2024-01-01T16:13:26
updated: 2024-03-22T09:10:47
---
- [[0010 Programming 👩‍💻]]
___

## 아직 push하지 않은 여러 커밋들의 메시지를 바꾸고 싶어요

<https://stackoverflow.com/a/1186549>

`bbc643cd` 커밋부터 쭉 커밋 이력들을 다시 보고자 한다면 다음 명령어를 치면 된다.

```shell
git rebase --interactive bbc643cd~
```

그리고 나타난 에디터에 기본적으로 적혀있는 커밋들의 `pick`을 `edit`으로 바꾸자. 저장하고 나가면 에디터가 다시 열리면서 순서대로 커밋메시지들을 수정할 수 있게 된다.
