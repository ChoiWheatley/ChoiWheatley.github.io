---
description:
aliases: 
tags: 
created: 2023-04-28T09:13:40
updated: 2023-07-15T21:33:02
title: 회원가입 페이지 작성하기 estsoft
---
- [notion assignment requirement](https://paullabworkspace.notion.site/9f94fb9829d74db9b6961484aaf97a8b)
- [결과물 코드 - join.html](https://github.com/ChoiWheatley/ormi-master/blob/main/ormi-2023-04-28/layout/join.html) | [join.css](https://github.com/ChoiWheatley/ormi-master/blob/main/ormi-2023-04-28/layout/join.css) |   ![[Pasted image 20230428173142.png]]
- "HTML/CSS 실습" 이라 적혀있는 헤더는 지금은 무시하고, 가운데 있는 회원가입 파트만 눈여겨 보자.

# what i had learned

- `<fieldset>`과 `<legend>` 
	- 여러가지 필드를 묶어서 `<fieldset>`을 제공하고, 그 제목 태그를`<legend>` 라고 부른다. 
- `<label>` 과 `<input>` 은 각각 `for` 과 `id` 로 매치가 된다.
- `<legend>`를 원하는 위치에 넣고 싶다면, 불편하지만 CSS에서 `fildset { position: relative; }` 속성을 넣고 `legend { position: absolute; }`를 넣어주면 된다. [MDN position](https://developer.mozilla.org/en-US/docs/Web/CSS/position) 에 적힌 여러 위치지정 속성 `bottom`, `left`, 등등을 활용하여 원하는 위치에 직접 박아넣을 수 있게 된다.
- 
