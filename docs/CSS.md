---
description:
aliases: 
tags: 
created: 2023-04-26T21:43:08
updated: 2023-07-11T15:21:09
title: CSS
---
- [CSS 기본 notion src](https://paullabworkspace.notion.site/CSS-bce6b95b464d4c978b3de1e60eb32d6e)
- [CSS 구조 notion src2](https://paullabworkspace.notion.site/CSS-63885a03f5c740d1952a709faacceb93)
- [CSS 상속 notion src3](https://paullabworkspace.notion.site/CSS-edb06c0a7b0d4a519375a68c2a56d5c7)
- [CSS selector notion src4](https://paullabworkspace.notion.site/CSS-70052fc35e734f4a86f0d4d4f85090f2)
- [30분 안에 끝내는 Bootstrap youtube](https://youtu.be/2znzBerWyWU)
## Structure
![[image-2.png]] ^nbm2ta
	- header(selector)
	- decl block
	- property
	- property value
- inline style
- inside `head` tag, separated `style` tag, external style with `link` 
	- `<link rel="stylesheet" href="./style.css">`
- Inheritance
	- style `tag`
		- `h1 { ... }`
	- style `group selector`
		- `h1,h2,p { ... }`
	- style with inheritance
		- `div { ... }` => 모든 `div` 태그의 하위 요소들도 저놈의 저시기를 따라간다.
	- inheritability
		- color => Ok
		- width, height, margin, padding, border => No
		- but, you can override any property with keyword `inherit`
		- you can follow default style defined by browser with keyword `initial`

- [?] `background`와 `background-color`의 차이가 무엇인가요?
	- `background` $\supset$ `background-color` 
	- 그래서 background만 설정하면 내가 따로 뭘 적지 않아도 추가적인 스타일이 로드된다.  
	- ![[Pasted image 20230427111746.png]] 

## Selector
- universal selector `*`
- type selector => such as tags, elements
- id selector `#` => has `id` attr, which **MUST** be unique in a whole document
- class selector `.` => can be duplicated unlike id selector.
- attr selector `[]` => any elements that has specified attr will be affected
	- `[type="button"] { ... }`
- group selector `,` 
- complex selectors (Combinators)
	- [descendant combinator ` `](https://developer.mozilla.org/en-US/docs/Web/CSS/Descendant_combinator) (single space character) => all descendants (children of children)
	- child selector `>` => direct children
	- [sibling combinator `~`](https://developer.mozilla.org/en-US/docs/Web/CSS/General_sibling_combinator) => element which is on same path as specified => not necessarly immediate
	- [djacent sibling combinator `+`](https://developer.mozilla.org/en-US/docs/Web/CSS/Adjacent_sibling_combinator) => matches the second element only if it immediately follows the first element the first element
- [?] MDN 문서를 보니까 Selector와 Combinator가 따로 구분되어있는데 무슨 차이를 가지고 있나여
	- [x] https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors/Combinators 이거 읽으셈
	- combinator는 selector인데, 여러개의 selector를 조합했다고 이름이 그렇게 붙은거다.
- pseudo class selector
	- URL 클릭하면 저시기 색깔 바뀌는거 있져 그거 지정하는 선택자를 일컫는다.
	- `:link` => not visited
	- `:visited`
	- `:hover` 
	- `:active`
	- `:focus`
- structural pseudo class selector
	- `:first-child`
	- `:last-child`
	- `:nth-child`
	- `:not`
- [?] pseudo class와 pseudo element의 차이점이 아직 잘 몰루?겠어요
	- [x] https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors/Pseudo-classes_and_pseudo-elements 읽어보셈
	- 읽어보니까 일단 눈에 바로 보이는 차이는 `:` 과 `::`임; 전자가 pseudo class이고, 후자가 pseudo element임.
	- pseudo class는 우리가 수업시간에 배웠던 대로 마우스 `hover`와 같이 특정한 상황(상태)에서만 나타나는 선택자인 것이다. 따라서 링크를 클릭했는가 아닌가에 따라서 `link`와 `visited`가 있는거지.
	- 반면에 pseudo element는 그 자체로 새로운 HTML 요소를 마크업에 추가해준다고 한다.
		- `::first-line` pseudo-element-selector는 보이는 화면에 따라서(윈도우 크기라던가) 오직 첫번째 줄만을 선택해준다.
		- `::before` 은 CSS 안에 작성한 HTML 문서를 반드시 선택된 요소 **전에** 박아넣는다.
		- `::after`는 그럼 더 설명 안해도 되겠지? ㅎㅅㅎ


## Selector Priority Rules
- can be overrided => later overwrite prior one
- can be selected by most specific selector
	- `*` $\lt$ 
	- type, pseudo element selector $\lt$ 
	- `.` , `[]`, pseudo class selector $\lt$ 
	- `#` $\lt$
	- inline style
- 가중치 점수가 아무리 높더라도 유형 선택자 가중치 법칙을 이길 수 없다.

## Display Attributes
- can change *block* and *inline* attribute into custom one
- `block`
- `inline`
- `inline-block`
![[Pasted image 20230427143822.png|450]]
- `flex`
- `grid`
- `none` => can be used as modal, menu

## Units
- `px`
	- absolute pixels
- `%`
	- parent 크기를 기준으로
- `em`
	- parent로부터 받은 상속받은 요소의 ==글자크기== 를 기준
- `rem`
	- root 글자크기를 기준으로 (16px)
- `vw`, `vh`
	- 뷰포트 너비, 높이를 기준으로 하는 퍼센트 단위
	- [!] `%`는 부모 요소를 기준으로 설정이되기 때문에 `vw`, `vh`와 동일하게 하려면 CSS 상에서 reset을 해야 한다. HOW?
		-  `body { margin : 0 }`


# flex
- [flexbox froggy 플렉스 연습 게임](https://flexboxfroggy.com/#ko)
- [flex grid 플렉스 시뮬레이터](https://flexngrid.com/)
- [MDN flex 문서](https://developer.mozilla.org/ko/docs/Learn/CSS/CSS_layout/Flexbox)
- [[회원가입 페이지 작성하기 estsoft]] 를 수행하다가 `legend`와 `fieldset`이 아주 이상했다. 이 문제를 해결해 줄 수 있는 녀석이 바로 `flex`이다.
- 자식요소(`flex-item`)들이 컨테이너(`flex-container`) 안 공간을 *알아서* 배치하게 만들어준다.
- 화면이 좁아지면 가로로 배치되던 요소들이 세로로 배치되는걸 의미
- 주축을 지정한다. (단순히 가로,세로가 아님)

# float
- 신문에서 보면 가끔 그림의 테두리 따라 글씨들이 꽉 채워지는 것을 볼 수 있다. 
- flow처럼 흐르는 CSS 문서의 규칙을 떠나 별도로 적용이 되는 규칙이며, 인라인 요소가 그 주위를 감싸도록 한다.
- https://developer.mozilla.org/ko/docs/Web/CSS/float
- 문제는, 화면 크기조절을 할 시에 화면 깨짐이 빈번하다고 주의를 주었다.



---

# 개꿀팁
- [[css - aspect-ratio를 사용하여 원하는 비율로 맞추기]]