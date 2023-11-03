---
description:
aliases: 
tags: 
created: 2023-04-27T18:04:38
updated: 2023-07-25T10:40:25
title: vim 관련 꿀팁
---

# Cheatsheet

- `A` 마지막 문장에 커서 위치시키고 편집모드로 들어가기 | [읽어보기](https://vi.stackexchange.com/questions/4307/multiple-cursors-at-desired-location)
- `.`: 방금 수행한 명령 반복하기 | [읽어보기](https://vi.stackexchange.com/questions/4307/multiple-cursors-at-desired-location)
- `\%V` visual block에 대하여만 search, replace 수행하기: `'<'>s/\%V{pattern}/{string}/g` | [읽어보기](https://vim.fandom.com/wiki/Applying_substitutes_to_a_visual_block)
- `q <letter>`로 키 스트로크 record, 사용할 때 `@ <letter>`를 사용 | [읽어보기](https://stackoverflow.com/questions/1527784/what-is-vim-recording-and-how-can-it-be-disabled)
- `zz`, `zt`, `zb` : 현재 커서를 기준으로 중간, 상단, 하단으로 에디터 옮기기
- `ci[`, `di[` : 각각 `[]` 안에 있는 내용을 지우고 편집모드, 지우기 `[` 자리에 `{`, `(`, 
	- 심지어 html인 경우에는 `t`(tag)도 가능함. 
	- 심지어 python인 경우에는 `i`(indent)도 가능함.
	- `i` 대신에 `a`를 넣으면 괄호까지 삭제 범위가 넓어짐.

# plugins

- [vim surround](https://github.com/tpope/vim-surround)  
	`cs"'` => `"hello"`를 `'hello'`로 바꿔줌.

# 읽을거리

- [[vim indentwise, motions based on indent depts or levels in normal, visual, and operator-pending modes]]
- [[Easymotion Vim cheatsheat]]

<iframe src="http://www.fprintf.net/vimCheatSheet.html" allow="fullscreen" allowfullscreen="" style="height:100%;width:100%; aspect-ratio: 16 / 9; "></iframe>
## Advanced

- [[vim selectively find and replace text]]