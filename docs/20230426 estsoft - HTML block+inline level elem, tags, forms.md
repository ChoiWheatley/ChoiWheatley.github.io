---
description:
aliases: 
tags: 
created: 2023-04-26T09:06:47
updated: 2023-07-15T21:30:22
title: 20230426 estsoft - HTML block+inline level elem, tags, forms
---
- [notion site in main site](https://paullabworkspace.notion.site/HTML-CSS-91665f909bd44cb2ad8e6720b9da400e)
- [notion site for live class - HTML/CSS](https://paullabworkspace.notion.site/HTML-CSS-e59ccad043e84513860c9bf42a7cd49f) ^ylilpg
- [snippet generator](https://snippet-generator.app/)

# Glossary

- URI
	- Uniform Resource Identifier: 리소스를 구별하기 위해 만든 식별자.
	- 스킴으로, 해당 식별자가 담을 수 있는 속성들에 대한 정보를 갖고 있다.
- URL
	- Uniform Resource Locator: 리소스를 구별하는 것 뿐만 아니라 해당 리소스에 접근하기 위한 구체적인 정보를 갖고 있다.
	- URI $\supset$  URL

# Before getting started

- 프론트 쪽에서 HTML,CSS 쪽은 난이도가 높은 작업.
- 백엔드니까 오직 3일 짧고 굵게 배운다.
- 폴더이름, 파일이름 컨벤션
	- 공백 없이 영문 소문자, 언더바 `_` 대신 하이픈 `-` 사용하기(kebab case...)
- `index.html` 기준으로 항상 먼저 찾게된다. 그 파일을 기준으로 경로를 찾아가게 될 것이다.
- html validation : https://validator.w3.org
- empty element : 닫는 태그가 없는 태그 `<br>`
- comment : `<!-- comment -->`

# HTML standard

- html standard
	- `! + Tab` 해서 나오는 기본문서 뜯어보기 작성하지 않아도 그냥 브라우저가 대충 띄워주기는 하거든? [[20230426 estsoft - HTML block+inline level elem, tags, forms#^m5z19c]]
	- `<!DOCTYPE html>` : html 표준문서임을 의미함 하위 호환성 모드가 꺼진다.
	- `<html lang="en">` 이걸보고 가끔 크롬이 번역할거냐고 물어본다. 우리나라는 `ko-KR` 이지롱
	- `head` 메타데이터를 담는다.
		- `meta` 메타데이터 태그로, 각종 속성의 키, 값을 저장할 수 있지롱
			- `charset` 속성: 문자 인코딩의 종류를 설정. `utf-8`을 사용하여 전 세계 언어를 지원하도록 하는 것이 원칙.
			- `http-equiv="X-UA-Compatible" content="IE=edge"` => 망할 IE와의 호환성을 위해 작성
			- `name="viewport"` (얘는 도대체 왜 attr가 name이냐) `content` 속성에 width, height 값을 지정해 줄 수 있음. `content=initial-scale`은 처음 페이지가 로드 될 때 확대 축소 수준을 제어.
		- `title` 타이틀
		- `link` 현재 문서와 외부 리소스 (CSS, Font, Favicon)과 연결할 때 사용함. [[20230426 estsoft - HTML block+inline level elem, tags, forms#^jsa6sp]]

# block level & inline level element

- 기본적으로 모든 요소는 네모낳게 생겼다고 가정한다. => block이라고 부르고 이 블럭들이 쌓여 저시기 된다.
- `div` 태그를 사용하면 저시기가 세로로 작성된다. 세로로 작성되는 녀석은 **블록**요소이다.
- `span` 태그를 사용하면 저시기가 가로로 작성된다. 가로로 작성되는 녀석은 **인라인** 요소이다.
- Block element [[20230426 estsoft - HTML block+inline level elem, tags, forms#^6492bd]]
	- 가능한 부모의 모든 너비를 차지하게 된다. 
	- 블록 요소 안에 인라인 요소는 들어갈 수 있지만 인라인 요소 안에 블록 요소가 들어갈 수 없다. 예외적으로 `<a>`  인라인 요소는 태그는 인라인 안에 블록 요소 중첩이 가능함.
	- CSS를 사용하여 width, height 크기를 지정할 수 있고 Padding, Border, Margin 속성을 사용할 수 있다. 
- Inline element
	- 블록 $\supset$ 인라인 
	- 가로로 작성된다는게 컨텐츠의 흐름을 끊지 않는다는 말뜻임. 
	- CSS를 사용하여 Width, Height 크기를 지정할 수 **없다**. Padding, Border, Margin 속성 중 상하 Margin 속성은 사용할 수 **없다**.
- [headings map](https://addons.mozilla.org/en-US/firefox/addon/headingsmap/?utm_source=addons.mozilla.org&utm_medium=referral&utm_content=search) 를 사용하면 목차를 볼 수 있다!!! [[20230426 estsoft - HTML block+inline level elem, tags, forms#^84gubn]]  
cheatsheet below  

|  | block | inline | inline-block |
| --- | --- | --- | --- |
| 요소 포함 | 인라인 요소 포함 가능 | 블록 요소 포함 불가(a 태그만 가능) | - | 
| 줄바꿈 | O(세로로 쌓임) | X(가로로 쌓임) | X(가로로 쌓임) |
| width, height | O | X | O |
| padding | O | X | O |
| margin | O | △ (left,right만 적용 / top,bottom 적용 X) | O |

| border | O   | O   | O   |
| ------ | --- | --- | --- 
|

# Tags

- 다양한 태그가 있지만, 여기에 적는 건 의미가 별로 없는 것 같다. 그때그때 찾아가면서 작성하자. [실습링크](https://github.com/ChoiWheatley/ormi-2023-04-26/blob/3ea3198848a705061d0436b1a6b64ef05a9022d6/002.html) 
- [`img`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Img)
	- inline, `style="display: block;"` 으로 블록요소처럼 떨어지게 만들 수 있다.
	- attr
		- `width`
		- `height`
		- `style` CSS 속성을 넣을 수도 있음
		- `alt` alternative text, 파일 로딩이 안 됐을때 표시하는 대체 텍스트. 스크린리더 지원.
- [`a`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a)
	- inline
	- attr
		- `href` link's destination
		- `target` where to display this URL
			- `_self` default, current browsing context
			- `_blank` new tab
			- `_parent` parent browsing context of the current one

# Forms

- `form`
	- 반드시 `<input type="submit">` 태그와 연동이 되어야 제출이 가능하다.
	- `method`::POST 방식
		- 양식 데이터를 **별도로** 요청 본문으로 전송
		- 캐시없음
		- 길이제한없음
		- GET방식보다는 보안성 있음.
	- `method`::GET 방식
		- name, value 쌍을 사용하여 쿼리문에 추가
		- 브라우저 히스토리, 캐시에 남음.
		- 길이제한있음
		- 보안 meh
	- `action` 
		- 양식 데이터를 처리할 프로그램의 `URI`를 명시
		- 해당 속성을 비워두면 이 데이터는 `form` 태그가 있는 페이지로 보내진다.
	- `label`
		- `for` -- `id` attr의 값을 동일하게 만들어 라벨과 인풋을 연결
		- 또는 중첩하여 연결할 수도 있음.
	-  `filedset` 여러개의 인풋을 묶고 싶을 때 사용
	- `legend`를 사용하면 form에 제목을 달 수 있음.
	- `value=button`은 별도의 레이블이 필요하지 않음.
- `button`
	- attr
		- `button` => 얘가 없으면 GET 방식으로 날아간다. 무조건 JS와 연동하여 사용.
		- `submit` => 서버로 양식 데이터를 전송
	- [!] `a` 태그와 `button` 태그의 차이점을 설명하세요
		- `a`는 특정 `URL`로 이동하기 위한 목적만을 가지고 있다.
		- `button`은 JS단에서 이동 말고도 모달 띄우기 등 다른 여러 행동에 연결시킬 수 있다.
- `input`
	- attr
		- `type`
			- `button`
			- `submit`
				- `form`의 `action`이 실행되기 위한 조건이 제출인데, 그 제출을 담당한다.
			- `reseet`
			- `text`
			- `password`
			- `email`
			- `search`
			- `tel`
			- `number`
			- `checkbox`
			- `radio`
				- `name`을 같게 만들어야 여러 라디오박스 중 하나만 클릭할 수 있게 만들어준다.
			- `file`
				- `accept` 허용하는 파일 유형을 제한
				- `multiple` 여러 개의 파일을 선택가능
			- `date`
			- 기타등등
		- `name`
		- `value`
		- `autocomplete`
		- `placeholder`
		- `required`
		- `disabled`
		- `readonly`
  - 않이... `<button type"button">`이랑 `<input type="button">` 은 뭐임... 뭘 써야하냐?? 
	- `<button type="button">`을 선호한다. `input` 태그는 빈 요소라서 텍스트밖에 지정할 수 없다. 자율도에 있어서 button 태그가 더 좋다. [[20230426 estsoft - HTML block+inline level elem, tags, forms#^y743qo]]



---

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```

^m5z19c

```html
<html>
<head>
  <!-- 스타일 시트 링크 -->
  <link rel="stylesheet" href="style.css">
  
	<!-- 폰트 링크 -->
  <link rel="stylesheet" href="font.ttf">
  
	<!-- 파비콘 링크 -->
  <link rel="shortcut icon" href="favicon.ico"> 
</head>
<body>
</body>
</html>
```

^jsa6sp

```html
<body>
    nyancat
    <div>div는 </div>
    <div>세로로</div>
    <div>쌓입니다.</div>
    <span>스판은</span> <span>가로로</span> <span>쌓입니다.</span>
</body>
```

^6492bd  
![[Pasted image 20230426113227.png]] ^84gubn

```html
<input type="button" value="버튼">
<button type="button">버튼</button>
```

^y743qo
