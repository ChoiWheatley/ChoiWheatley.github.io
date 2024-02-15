---
aliases: 
tags: 
description:
title: wine setting
created: 2024-02-15T18:28:56
updated: 2024-02-15T19:30:12
---
- [[linux 🐧]]
---

## 와인으로 실행한 카카오톡의 한글이 네모네모 병에 걸렸어요

- <https://forcecore.tistory.com/1320>

`.bashrc` 혹은 `.zshrc` 파일에 적어놓았던 환경변수 `LANG="ko_KR.utf8"`에 오타를 낸 것이 첫 번째 문제의 원인이었다.

`wine regedit` 으로 터미널에 쳐서 레지스트리 편집기를 연다. MS Shell Dlg를 수정하면 되는 건가? `/home/choiwheatley/.wine/drive_c/windows/Fonts` 위치에 NanumGothic을 집어넣고 혹시 몰라 Arial, Courier New, Times New Roman 과 같은 폰트들 또한 같이 넣어주었다.

![[Pasted image 20240215192929.png]]

해도해도 안되길래 모든 밸류들을 NanumGothic으로 바꿔봐도 시스템 글꼴이 네모네모 현상이 발생하는 것은 막을 수 없었다고 한다.
