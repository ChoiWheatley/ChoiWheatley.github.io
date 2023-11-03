---
aliases: 
tags: 
description:
title: sequelize, a MySQL ORM for javascript
created: 2023-11-03T19:47:03
updated: 2023-11-04T07:46:44
---
- [[0018 Javascript ☕️]]
- [[express.js]]
- <https://www.npmjs.com/package/sequelize>
- <https://sequelize.org/api/v6/identifiers>
___

## 설치 및 마이그레이션

```shell
npm install sequelize # js 파일 안에서 사용하기 위한 패키지
npm install mysql2 # sequelize가 의존하는 패키지

# sequelize-cli, nodemon 라이브러리를 DevDependency로 설치합니다.
npm install -D sequelize-cli  # npx와 함께 db 생성, 모델 생성, 마이그레이션 생성을 자동화
npm install nodemon # hot reloading

# 설치한 sequelize를 초기화 하여, sequelize를 사용할 수 있는 구조를 생성합니다.
npx sequelize init 
```

`config/config.json` 파일을 수정하여 데이터베이스를 연동한다. 아래 명령줄을 실행하여 새 데이터베이스를 만든다.

```shell
npx sequelize db:create
```

아래 npx 명령줄을 실행하여 `Posts` 모델에 대한 마이그레이션 파일을 생성한다. 얘가 있어야 실제 DB에 테이블을 만들어낼 수 있음. [sequelize 문서](https://sequelize.org/docs/v6/other-topics/migrations/)

```shell
npx sequelize model:generate --name Posts --attributes title:string,content:string,password:string
```

아래는 이제 실제 마이그레이션을 진행하는 코드. migration 파일과 MySQL 테이블을 매핑한다.

```shell
npx sequelize db:migrate
```

### 🧩  Sequelize CLI 간단하게 살펴보기!

<https://github.com/sequelize/cli#usage>

- [sequelize db:create](https://github.com/sequelize/cli#usage)
	- config/config.json에 설정한 database를 생성합니다.
- [sequelize db:drop](https://github.com/sequelize/cli#usage)
	- config/config.json에 설정한 database를 DROP합니다.
- [sequelize model:generate](https://sequelize.org/docs/v6/other-topics/migrations/#creating-the-first-model-and-migration)
	- migration과 model을 생성합니다.
	- 각 Column의 속성을 지정해줄 수 있습니다.
- [sequelize db:migrate](https://sequelize.org/docs/v6/other-topics/migrations/#running-migrations)
	- migrations 폴더에 있는 migration 파일을 이용해 MySQL의 테이블을 생성합니다.
- [sequelize db:migrate:undo](https://sequelize.org/docs/v6/other-topics/migrations/#undoing-migrations)
	- 가장 최근에 실행된 db:migration 명령을 되돌립니다.
- [sequelize seed:generate](https://sequelize.org/docs/v6/other-topics/migrations/#creating-the-first-seed)
	- seeder 폴더에 있는 seed 파일을 이용해 각 테이블에 데이터를 삽입합니다.

### ❓ createdAt의 defaultValue가 생각했던것과 달라요!

MySQL의 경우 defaultValue를 현재 시간(`CURRENT_TIMESTAMP`)로 등록하려 할 때, `Seqeulize.NOW`로 속성을 입력할 경우 정상적으로 반영이 되지 않는 문제가 있습니다. 

해당하는 문제를 해결하기위해 `defaultValue: Sequelize.fn("now")`로 설정하였습니다. [sequelize GH 참고](https://github.com/sequelize/sequelize/issues/4679)

### `sequelize-cli` 사용하지 않고 마이그레이션 하기

cli 말고 일반 패키지 `sequelize`에서도 테이블 생성기능이 존재하다. 

```js
// app.js
const { sequelize } = require("./models/index.js");

async function main() {
  // sequelize에 테이블들이 존재하지 않는 경우 태이블을 생성합니다.
  await sequelize.sync();
}

main();
```
