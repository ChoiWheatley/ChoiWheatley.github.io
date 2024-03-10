---
aliases: 
tags: 
description:
title: cross-env {NodeJS}
created: 2024-03-10T00:22:20
updated: 2024-03-10T00:28:35
---
- <https://www.npmjs.com/package/cross-env>
---

cross-env는 여러 운영체제 (Unix 기반, 윈도우즈)에서 공통된 방식으로 환경변수를 세팅한 채로 nodejs 프로그램을 실행시키기 위해 나왔다. 

`.package.json` 파일에 새 스크립트를 추가한다.

```json
{
	"scripts": {
		"build": "cross-env NODE_ENV=production webpack --config build/webpack.config.js",
		"foo": "cross-env FIRST_ENV=one SECOND_ENV=two node ./my-program"
	}
}
```

실행할 땐 익히 아는 그 명령어를 쓰면 된다.

```
npm run build
npm run foo
```
