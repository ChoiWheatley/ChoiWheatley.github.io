---
description:
aliases: 
tags: 
created: 2023-05-01T13:07:05
updated: 2023-07-15T21:33:02
title: 타입 - python
---
- 변수타입
	- `list` 는 내부 자료값들의 변경이 가능하다. 하지만 `tuple`은 불변임.
	- `del`은 데이터를 지울 때 사용가능. `del some_dictionary['some-key']`
- 변수속성 `dir(<variable>)` => 메서드 이름들 전부 나열하기
	- [ ] 던더 함수(매직 메서드)?
- 형 변환
	- `<target-type>(<variable>)`
- 출력을 위한 여러가지 방법 `print(type(<variable>))`
	- f-string 용법 since python ver 3.6
	- r-string 용법: raw string이라서 `\n` 같은거 사용할 수 있음.
