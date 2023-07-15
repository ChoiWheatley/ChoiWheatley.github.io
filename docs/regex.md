---
description:
aliases: 
tags: 
created: 2023-05-15T14:07:54
updated: 2023-07-15T21:33:03
title: regex
---
- [실습 링크](https://regexr.com/5nvc2)
- [인프런 정규표현식: python으로 톺아보기](https://www.inflearn.com/course/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%A0%95%EA%B7%9C%ED%91%9C%ED%98%84%EC%8B%9D/dashboard)]
- [notion link](https://paullabworkspace.notion.site/1c57fc683c33468d95e7a490b6f66c95)

- `?` 
	- 0개 이거나 1개 이거나
	- `010-8752-4037` 과 `01087524037`을 모두 검출하고 싶다면
	- `010?8752?4037` 
- Anchors `^` & `$`  
		- `^hello` : 처음 만나는 hello  
		- `hello$` : 마지막에 만나는 hello
- `.` 
	- all occurences
- Character Classes
	- `\w` : 단어
		- ![[Pasted image 20230515150429.png]]
	- `\w{5}` : 5개의 글자와 공백 하나
	- `\W` 단어가 아님(?) ==> 알파벳 & `_`
	- `\d` : 숫자
	- `\D` : not 숫자
	- `\s` : 공백문자
	- `\S` : not 공백문자
- `[]` 
	- optional occurences
	- `[Hh]ello` ==> hello, Hello
	- 구간
		- `[0-9]`
		- `[a-zA-Z]`
	- 반복 (수량자)
		- 예를 들어 전화번호를 쿼리하는 정규표현식은 다음과 같이 작성할 수 있다.
		- `010[-,. ]?[0-9][0-9][0-9][0-9][-,. ]?[0-9][0-9][0-9][0-9]`
		- 하지만 `[0-9]`가 너무 자주 오는 것 같은데 => `{n}`을 사용하여 반복을 줄일 수 있다.
		- `010[-,. ]?[0-9]{4}[-,. ]?[0-9]{4}`
		- `{3}` 3개
		- `{3,}` 3개 이상
		- `{1, 3}` 1~3개
	- 부정
		- 대괄호 안에 들어있는 `^`(caret)은 부정의 의미.
		- `[^a-zA-Z0-9]` : 대괄호 안에 명시된 패턴을 제외한 나머지 문자열을 찾는다.
- 서브그룹
	- `(on|ues|rida)` :  `|`를 기준으로 여러개를 묶는다.
- flags : 정규표현식 맨 마지막에 `/`를 붙이고 뒤에 키워드를 붙임
	- `g`lobal
	- case `i`nsensitive
	- `m`ultiline
	- `s`ingle line
