---
aliases: 
tags: 
description:
title: fzf, fuzzy finder for terminal
created: 2023-09-07T20:54:43
updated: 2023-09-07T20:59:59
---
<https://github.com/junegunn/fzf>  

심지어 한국인이 만든 커맨드 라인 검색기기. 그 자체로는 검색같은 걸 하지는 못하는데, 다른 명령어 (`fd`, `find`, `ls`)의 결과를 `|` 파이프 연산으로 물려서 쓰면 이보다 좋을 수 없다.

가장 많이 쓰게 될 건 역시 파일 검색일 것이다. 대표적인 명령어를 소개하자면,

```shell
find | fzf
```
