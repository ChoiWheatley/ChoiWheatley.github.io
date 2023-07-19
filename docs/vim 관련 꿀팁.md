---
description:
aliases: 
tags: 
created: 2023-04-27T18:04:38
updated: 2023-07-20T05:43:55
title: vim 관련 꿀팁
---

# Cheatsheet

- `A;` 마지막 문장에 `;` 삽입하기 | [읽어보기](https://vi.stackexchange.com/questions/4307/multiple-cursors-at-desired-location)
- `.`: 방금 수행한 명령 반복하기 | [읽어보기](https://vi.stackexchange.com/questions/4307/multiple-cursors-at-desired-location)
- `\%V` visual block에 대하여만 search, replace 수행하기: `'<'>s/\%V{pattern}/{string}/g` | [읽어보기](https://vim.fandom.com/wiki/Applying_substitutes_to_a_visual_block)
- `q <letter>`로 키 스트로크 record, 사용할 때 `@ <letter>`를 사용 | [읽어보기](https://stackoverflow.com/questions/1527784/what-is-vim-recording-and-how-can-it-be-disabled)

# plugins

- [vim surround](https://github.com/tpope/vim-surround)  
	`cs"'` => `"hello"`를 `'hello'`로 바꿔줌.

# 읽을거리

- [[vim indentwise, motions based on indent depts or levels in normal, visual, and operator-pending modes]]
- [[Easymotion Vim cheatsheat]]

<iframe src="http://www.fprintf.net/vimCheatSheet.html" allow="fullscreen" allowfullscreen="" style="height:100%;width:100%; aspect-ratio: 16 / 9; "></iframe>
