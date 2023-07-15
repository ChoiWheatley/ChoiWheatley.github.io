---
description:
aliases: 
tags: 
created: 2023-06-01T09:22:04
updated: 2023-07-15T21:33:04
title: jQuery
---
[jQuery & Ajax {Notion}](https://www.notion.so/jQuery-Ajax-22-5-60ddc1d3731542baa9e888bd5a03dcff)

생애주기 고점을 지나 쇠락기에 지나고 있다. 여러곳에서 얘를 빼고는 있지만 아직도 굳건한 랭킹 2,3위를 차지하고 있다.

장점: pure JS의 번잡스러움을 단칼에 쳐냈다. 하루면 배울 수 있어 매우 효율적임.

- `$` => 얘 자체가 함수임. 함수 인자로 선택자를 사용한다.
- `click(fn)` => 누르면 안에 함수를 실행한다. ![[Pasted image 20230601093517.png]]
- `css('color', 'red')`
- `html([html 코드])` : 걍 HTML 문서를 가져오거나 치환한다.
- `hide()` : 숨기기
- `val([hello])` 값 쓰거나 가져오기
- `text([value])` input 텍스트 쓰거나 가져오기

### filters

### effects

- $("p").hide();
- $("p").show();
- $("p").toggle();
- $("#div1").fadeIn();
- $("#div1").fadeOut();
- $("#div1").fadeToggle();
- $("#div3").fadeIn(3000); // 모달창, 이미지 갤러리 등에서 사용

### events

- click

```js
// JQuery - animate
// 버튼 클릭했을 때 변화주기
// 'slow' 대신 ms 단위로도 줄 수 있습니다.
$('#btn_ani').click(function() {
  $('#box').animate({
    width: '300px',
    height: '300px',
    opacity: 1,
  }, 'slow');
});
```

- hover

```js
//hover - 색 변경하기(호버 되었을 때 yellow, 호버에서 벗어났을 때 blue)
$("#box").hover(function(){
  $(this).css("background-color", "yellow");
}, function(){
  $(this).css("background-color", "blue");
});
```

- mouse event

```js
$("#p1").mouseenter(function(){
	alert("You entered p1!");
});
$("#p1").mouseleave(function(){
	alert("Bye! You now leaved p1!");
});
```
