---
aliases: 
tags: 
description:
title: vim Increasing or decreasing numbers
created: 2024-10-18T20:00:33
updated: 2024-10-18T20:07:15
---
- [vim.fandom.com](https://vim.fandom.com/wiki/Increasing_or_decreasing_numbers)
- [vimdoc#Adding and subtracting](https://vimdoc.sourceforge.net/htmldoc/change.html#CTRL-A)

---

## README

연속된 숫자를 복사 붙여넣기 하고 있으면 스트레스 받지 않는가? 이 경우 `Ctrl-A` 기능을 활용하면 자동으로 숫자를 인식해서 높이거나 줄일 수 있다.

```
example 1
example 2
example 3
example 4
example 5
example 6
example 7
example 8
example 9
example 10
example 11
example 12
example 13
example 14
example 15
example 16
```

## HOWTO

아무 숫자나 적혀있는 라인에 커서를 가져다 댄 뒤 (Normal mode) `Ctrl-A`와 `Ctrl-X` 를 눌러보자. 대문자 A와 X라는 점 참고바란다.

```
example 11
example 12 # Ctrl-A
example 10 # Ctrl-X
```

이 기능을 매크로로 활용하면 다음과 같다. 이때 처음 시작하는 라인의 아무곳에나 위치한 상태여야 한다:

- `qa`: a 레지스터에 매크로를 녹화한다.
- `Y`: 해당 라인을 복사한다
- `p`: 해당 라인을 다음 줄에 붙여넣기하고 커서를 새 라인으로 옮긴다.
- `Ctrl-A` 또는 `Ctrl-X`: 해당 라인에 있는 숫자를 증가 혹은 감소한다.
- `q` 매크로 녹화를 중지한다.

매크로를 한 번에 여러번 사용하려면 `@a` 앞에 숫자를 붙이면 된다.

```
example 1
example 2
example 3
example 4
example 5
example 6
example 7
example 8
example 9
example 10
example 11
example 12
example 13
example 14
example 15
example 16
```
