---
aliases: 
tags: 
description:
created: 2023-06-21T15:41:46
updated: 2023-07-15T21:33:04
title: FileReader {js}
---
- [mdn](https://developer.mozilla.org/en-US/docs/Web/API/FileReader)
- [bookstore에서 사진파일을 다루는 코드](https://github.com/ESTsoft-Book-Project/bookstore/blob/25379d55f885264568e12dd9d196658858a4fd86/templates/create_product.html)
- [[Javascript]]
- 사진파일도 읽을 수 있다. 아래의 스니펫은 이미지 데이터를 가져오는 부분이다.

```js
const fileInput = document.querySelector("#image");
const imageFile = fileInput.files[0];

const reader = new FileReader();
reader.onloadend = function () {
  const imageData = reader.result.split(",")[1];
  // ...
};

reader.readAsDataURL(imageFile);
```

GPT의 설명에 따르면...

`const imageData = reader.result.split(",")[1]`는 데이터 URL을 쉼표(,)로 분리하고, 두 번째 요소를 선택하여 이미지 데이터만 추출합니다. 데이터 URL은 `data:image/jpeg;base64,`와 같은 형식으로 되어 있으며, 실제 이미지 데이터는 이 형식의 두 번째 부분에 위치하므로 `[1]`을 사용하여 추출합니다.
