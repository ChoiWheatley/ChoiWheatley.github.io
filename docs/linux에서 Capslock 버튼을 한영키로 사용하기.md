---
aliases: 
tags: 
description:
title: linux에서 Capslock 버튼을 한영키로 사용하기
created: 2024-03-16T16:56:36
updated: 2024-03-16T17:01:21
---
- [[linux 🐧]]
- [참고한 블로그](https://ieworld.tistory.com/4) 이 블로그는 RAlt를 한영키로 바꾸었으나, 같은 디렉토리에 `capslock` 이라는 파일또한 존재하니 똑같이 생각하고 작성하면 되겠지
---

## Steps

1. 다음 파일을 연다: `/usr/share/X11/xkb/symbols/capslock`

	```
	default  hidden partial modifier_keys
	xkb_symbols "capslock" {
	    replace key <CAPS> { [ Caps_Lock ] };
	    modifier_map Lock { Caps_Lock };
	};
	
	...
	```

2. `"capslock"` 이라고 되어있는 부분의 `replace key <CAPS> { [] };` 에서의 대괄호 부분을 `[Hangul]`로 바꿔준다.

	```
	default  hidden partial modifier_keys
	xkb_symbols "capslock" {
	    replace key <CAPS> { [ Hangul ] };
	    modifier_map Lock { Caps_Lock };
	};
	
	...
	```
