---
aliases: 
tags: 
description:
title: jsonwebtoken npm
created: 2023-11-03T19:29:19
updated: 2023-11-06T16:14:38
---
- <https://www.npmjs.com/package/jsonwebtoken>
- [[express.js]]
- [[Securities about {https} and {jwt {cookie}, {session}}]]
- might depends on [[cookie-parser]] 
___

JWT 라이브러리.

```js
// 암호화
const jwt = require("jsonwebtoken");
const token = jwt.sign({ myPayloadData: 1234 }, "<PrivateKey>");
console.log(token); // eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJteVBheWxvYWREYXRhIjoxMjM0LCJpYXQiOjE2Njc1NjE0NDB9.nvYSsLsT8jp7IfkbB2seCNeuLqRBgrrzDjKRFXjvoUE

// 복호화
const decodedValue = jwt.decode(token);
```

JWT는 누구나 복호화가 가능하기 때문에 비밀번호 같은걸 저장하면 안된다. JWT의 목적은 페이로드가 변조됐는지 여부를 검사하기 위해 탄생했다는 거 명심

## Login API with JWT

```js
const express = require("express");
const JWT = require("jsonwebtoken");
const app = express();

app.post("/login", async (req, res) => {
	// 사용자 정보
	const user = {
		userId: 203,
		email: "chltmdgus604@gmail.com",
		name: "최승현",
	};
	// 사용자 정보를 JWT로 생성
	const userJWT = await JWT.sign(user, "<PrivateKey>", {expiresIn: "1h"});

	// Bearer 토큰 형식으로 userJWT에 할당
	res.cookie("sparta", `Bearer ${userJWT}`);
	return res.status(200).end();
});
```

## jsonwebtoken API

- 암호화

```js
const jwt = require("jsonwebtoken");
const token = jwt.sign(mydata, secretKey);
```

- 복호화

```js
const token = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJteVBheWxvYWREYXRhIjoxMjM0LCJpYXQiOjE2Njc1NjE0NDB9.nvYSsLsT8jp7IfkbB2seCNeuLqRBgrrzDjKRFXjvoUE";
const decodedValue = jwt.decode(token);
```

- sign

```js
app.post('/login', async (req, res) => {
  // 사용자 정보
  const user = {
    userId: 203,
    email: "archepro84@gmail.com",
    name: "이용우",
  }

  // 사용자 정보를 JWT로 생성
  const userJWT = await JWT.sign(user, // user 변수의 데이터를 payload에 할당
    "secretOrPrivateKey", // JWT의 비밀키를 secretOrPrivateKey라는 문자열로 할당
    { expiresIn: "1h" } // JWT의 인증 만료시간을 1시간으로 설정
  );

  // userJWT 변수를 sparta 라는 이름을 가진 쿠키에 Bearer 토큰 형식으로 할당
  res.cookie('sparta', `Bearer ${userJWT}`);
  return res.status(200).end();
});

app.listen(5002, () => {
  console.log(5002, "번호로 서버가 켜졌어요!");
});
```
