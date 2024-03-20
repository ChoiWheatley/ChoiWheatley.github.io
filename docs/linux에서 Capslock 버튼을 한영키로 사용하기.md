---
aliases: 
tags: 
description:
title: linux에서 Capslock 버튼을 한영키로 사용하기
created: 2024-03-16T16:56:36
updated: 2024-03-16T18:07:32
---
- [[linux 🐧]]
- [참고한 블로그](https://ieworld.tistory.com/4) 이 블로그는 RAlt를 한영키로 바꾸었으나, 같은 디렉토리에 `capslock` 이라는 파일또한 존재하니 똑같이 생각하고 작성하면 되겠지
---

## Steps

1. 다음 파일을 연다: `/usr/share/X11/xkb/symbols/capslock`
2. 아래에 아래와 같은 설정을 추가한다:

	```
	hidden partial modifier_keys
	xkb_symbols "Capslock To Hangul" {
		key <CAPS> { [ Hangul ] };
	};
	```

3. 다음 명령어를 터미널에서 입력하여 수정된 사항을 로드한다:

	```
	sudo dpkg-reconfigure xkb-data
	```
