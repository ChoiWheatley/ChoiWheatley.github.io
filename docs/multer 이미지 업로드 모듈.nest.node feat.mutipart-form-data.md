---
aliases: 
tags: 
description:
title: multer 이미지 업로드 모듈.nest.node feat.mutipart-form-data
created: 2024-05-06T14:30:22
updated: 2024-05-06T15:13:48
---

## multipart/form-data

> [!tldr] 텍스트 형식으로 인코딩 하는 기본 HTTP 전송방식에 바이너리 데이터를 허용하기 위해 만들어졌다.

multipart/form-data는 HTTP 요청에서 사용되는 컨텐츠 유형(Content-Type) 중 하나로, 여러 부분으로 구성된 데이터를 전송할 때 사용됩니다. 이 유형은 주로 파일 업로드와 같이 바이너리 데이터(binary data)를 포함하는 경우에 사용됩니다.

일반적으로 웹 폼(form)을 통해 파일을 업로드할 때 사용되며, 이러한 폼은 `<input type="file">`을 사용하여 파일 선택을 가능하게 합니다.

multipart/form-data 형식은 여러 부분(parts)으로 구성되며, 각 부분은 하나의 데이터 요소를 나타냅니다. 각 부분은 다음과 같은 구조를 가집니다:

1. **부분 헤더 (Part Header)**: 각 부분은 부분 헤더로 시작합니다. 이 헤더에는 Content-Disposition 및 Content-Type과 같은 헤더가 포함될 수 있습니다. Content-Disposition 헤더는 이 부분이 파일인지 폼 데이터인지를 나타내며, 파일의 이름과 함께 전송될 수 있습니다. Content-Type 헤더는 이 부분의 데이터 유형을 지정합니다.

2. **빈 줄 (Empty Line)**: 부분 헤더 다음에는 빈 줄이 따라옵니다.

3. **데이터 (Data)**: 이 부분의 실제 데이터가 여기에 옵니다. 이 데이터는 파일의 바이너리 데이터일 수도 있고, 폼 필드의 텍스트 데이터일 수도 있습니다.

multipart/form-data 형식을 사용하면 파일과 함께 텍스트 기반의 메타데이터를 전송할 수 있습니다. 이 메타데이터는 각 파일에 대한 추가 정보를 포함할 수 있습니다. 일반적으로 파일의 이름, 타입 등이 포함됩니다.

이러한 형식은 웹 애플리케이션에서 파일 업로드 및 관련 메타데이터 전송에 널리 사용됩니다. 파일 업로드를 지원하는 웹 서버는 이러한 요청을 받아서 적절히 처리하고 저장할 수 있습니다.

multipart/form-data를 사용하여 이미지 파일을 전송하는 경우, 데이터는 다음과 같은 형식을 갖습니다:

```http
Content-Disposition: form-data; name="image"; filename="example.jpg"
Content-Type: image/jpeg

(이미지 바이너리 데이터)
```

위의 예제에서 "image"는 폼 필드의 이름을 나타내며, "example.jpg"는 전송되는 파일의 이름입니다. "image/jpeg"는 전송되는 이미지의 MIME 타입을 나타냅니다.

비교적 다른 방법으로는 Base64 인코딩된 이미지 데이터를 직접 전송하는 것이 있습니다. 이 방법은 데이터를 텍스트 형식으로 인코딩하여 전송하므로 form-data 방식보다는 복잡하고 용량이 크지만, 데이터를 바로 전송할 수 있는 장점이 있습니다. 그러나 일반적으로 파일 업로드는 form-data 방식을 사용하는 것이 더 효율적입니다.

## multer

multer는 multipart/form-data를 처리하기 위해 사용되는 모듈이다. 바이너리 파일 업로드를 쉽게 처리할 수 있도록 도와준다.

```javascript
const express = require('express');
const multer = require('multer');
const upload = multer({ dest: 'uploads/' }); // 업로드된 파일이 저장될 경로

const app = express();

app.post('/upload', upload.single('image'), (req, res) => {
    // req.file은 이미지 파일의 정보를 담고 있는 객체입니다.
    console.log(req.file);
    res.send('File uploaded successfully.');
});

app.listen(3000, () => {
    console.log('Server is running on port 3000');
});
```
