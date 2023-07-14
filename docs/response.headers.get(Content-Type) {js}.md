---
aliases: 
tags: 
description:
created: 2023-06-29T14:24:39
updated: 2023-07-11T15:21:07
title: response.headers.get(Content-Type) {js}
---
- [mdn](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type)
- fetch를 사용해 받은 response가 JSON인지, HTML인지 판별할 수 있는 방법을 알아냈다. 바로 `Content-Type`을 이용하는 것이다. 이것은 대놓고 나와있지 않아서 찾기가 매우 힘들었다. 하지만 근성의 최승현! 😅 아래의 스니펫이 바로 response가 어떤 타입인지 해석하는 저시기이다.
- 참고할 것: `res.json()`, `res.text()` 를 호출하는 행위는 `res` 안의 상태를 바꾸기 때문에 최대 한 번밖에 호출할 수 없다.
```js
function errorHandle(res) {
	const contentType = res.headers.get("Content-Type").split(";")[0];
	if (contentType === "text/html") {
	
	  res.text().then(html => {
		document.body.innerHTML = html;
	  });
	
	} else if (contentType === "application/json") {
	
	  res.json().then(error => {
		  alert(error.message);

		}).catch((e) => {
		  alert("JSON 파싱 에러! 콘솔창을 확인하세요.");
		  console.log(e);
		})
	} else {
	  alert("알 수 없는 에러! 콘솔창을 확인하세요.");
	  console.log(e);
	}
}
```