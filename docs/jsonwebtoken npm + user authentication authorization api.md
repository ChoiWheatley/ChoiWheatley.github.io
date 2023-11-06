---
aliases: 
tags: 
description:
title: jsonwebtoken npm + user authentication authorization api
created: 2023-11-03T19:29:19
updated: 2023-11-06T20:45:37
---
- <https://www.npmjs.com/package/jsonwebtoken>
- [[express.js]]
- [[Securities about {https} and {jwt {cookie}, {session}}]]
- might depends on [[cookie-parser]] 
___

JWT 라이브러리.

JWT는 누구나 복호화가 가능하기 때문에 비밀번호 같은걸 저장하면 안된다. JWT의 목적은 페이로드가 변조됐는지 여부를 검사하기 위해 탄생했다는 거 명심

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

- [검사](https://www.npmjs.com/package/jsonwebtoken#jwtverifytoken-secretorpublickey-options-callback)

> Returns the payload decoded if the signature is valid and optional expiration, audience, or issuer are valid. If not, it will throw the error.

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

- login

```js
router.post("/login", async (req, res) => {
  // 사용자 정보
  const { nickname, password } = req.body;
  const user = await Users.findOne({ where: { nickname: nickname } });
  if (!user) {
    return res.status(404).json({ errorMessage: "없는 계정입니다." });
  }

  /// TODO - password 암호화 결과를 비교
  if (user.password !== password) {
    return res
      .status(400)
      .json({ errorMessage: "비밀번호가 일치하지 않습니다." });
  }

  // 사용자 정보를 JWT로 생성
  const userJWT = jwt.sign(
    user.toJSON(),
    "secretOrPrivateKey", /// TODO - secret key .env 파일에 저장하여 꺼내쓰기
    { expiresIn: "1h" } // JWT의 인증 만료시간을 1시간으로 설정 /// TODO - refresh token + expiresIn: 15min
  );

  // userJWT 변수를 sparta 라는 이름을 가진 쿠키에 Bearer 토큰 형식으로 할당
  res.cookie("sparta", `Bearer ${userJWT}`);
  return res.status(200).end();
});
```

- logout

```js
router.post("/logout", (req, res) => {
  res.clearCookie("sparta");
  res.sendStatus(200);
});
```

- signup

```js
router.post("/signup", async (req, res, next) => {
  const { nickname, password, passwordConfirm } = req.body;

  ///NOTE - Validation Logic
  ...

  try {
    /// TODO - password 암호화 결과를 저장
    const user = await Users.create({ nickname, password });
    res.status(201).json({ data: user });
  } catch (error) {
    console.log("💀", error);
    next(error);
  }
});
```
