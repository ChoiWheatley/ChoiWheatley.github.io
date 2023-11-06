---
aliases: 
tags: 
description:
title: bcypt npm
created: 2023-11-06T20:07:43
updated: 2023-11-06T20:14:19
---
- <https://www.npmjs.com/package/bcrypt>
___
비밀번호 해싱을 도와주는 라이브러리. 예제코드:

- encrypt

```js
const bcrypt = require('bcrypt');
const saltRounds = 10;
const myPlaintextPassword = 's0/\/\P4$$w0rD';
const someOtherPlaintextPassword = 'not_bacon';

// technique 1
bcrypt.genSalt(saltRounds, (err, salt) => {
	bcrypt.hash(myPlaintextPassword, salt, (err, hash) => {
		// Store hash in your password DB
	});
});

// technique 2
bcrypt.hash(myPlaintextPassword, saltRounds, (err, hash) => {
	// Store hash in your password DB
});
```

- check

```js
// Load `hash` from your password DB
bcrypt.compare("password", hash, (err, result) => {
	// result => true or false
});
```
