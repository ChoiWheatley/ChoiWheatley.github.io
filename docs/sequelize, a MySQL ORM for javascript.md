---
aliases: 
tags: 
description:
title: sequelize, a MySQL ORM for javascript
created: 2023-11-03T19:47:03
updated: 2023-11-06T01:04:52
---
- [[0018 Javascript ☕️]]
- [[express.js]]
- <https://www.npmjs.com/package/sequelize>
- <https://sequelize.org/api/v6/identifiers>
- [[Data Modeling {book-project}]]
___

## 설치 및 마이그레이션

- <https://sequelize.org/docs/v6/other-topics/migrations/>
- <https://sequelize.org/docs/v6/core-concepts/model-basics/>

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

아래 npx 명령줄을 실행하여 `Posts` 모델에 대한 마이그레이션 파일을 생성한다. 얘가 있어야 실제 DB에 테이블을 만들어낼 수 있음. 

```shell
npx sequelize model:generate --name Posts --attributes title:string,content:string,password:string
```

아래는 이제 실제 마이그레이션을 진행하는 코드. migration 파일과 MySQL 테이블을 매핑한다.

```shell
npx sequelize db:migrate
```

나중에 스키마를 바꾸고 싶을때가 온다면 마이그레이션 파일을 하나 새로 만든 뒤에 `db:migrate`를 하면 된다. 새 마이그레이션 파일을 생성하는 명령어는 다음과 같다.

```
npx sequelize migration:create --name <Name>
```

#### query interfaces

다음 [query interface {doc}](https://sequelize.org/docs/v6/other-topics/query-interface/) 문서를 확인하여 컬럼을 추가하거나 속성을 수정하는 등 다양한 수정을 해보자. 마이그레이션 파일을 새로 만들었다면 `up`에는 우리가 수정하고자 하는 것들을, `down`에는 원상복구를 하기 위한 작업을 작성해 넣는것이다!

- `createTable` ⟷ `dropTable`
- `addColumn` ⟷ `removeColumn`
- `changeColumn` 

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

```
Sequelize CLI [Node: 10.21.0, CLI: 6.0.0, ORM: 6.1.0]

sequelize <command>

Commands:
  sequelize db:migrate                        Run pending migrations
  sequelize db:migrate:schema:timestamps:add  Update migration table to have timestamps
  sequelize db:migrate:status                 List the status of all migrations
  sequelize db:migrate:undo                   Reverts a migration
  sequelize db:migrate:undo:all               Revert all migrations ran
  sequelize db:seed                           Run specified seeder
  sequelize db:seed:undo                      Deletes data from the database
  sequelize db:seed:all                       Run every seeder
  sequelize db:seed:undo:all                  Deletes data from the database
  sequelize db:create                         Create database specified by configuration
  sequelize db:drop                           Drop database specified by configuration
  sequelize init                              Initializes project
  sequelize init:config                       Initializes configuration
  sequelize init:migrations                   Initializes migrations
  sequelize init:models                       Initializes models
  sequelize init:seeders                      Initializes seeders
  sequelize migration:generate                Generates a new migration file      [aliases: migration:create]
  sequelize model:generate                    Generates a model and its migration [aliases: model:create]
  sequelize seed:generate                     Generates a new seed file           [aliases: seed:create]

Options:
  --version  Show version number                                                  [boolean]
  --help     Show help                                                            [boolean]

Please specify a command
```

### ❓ createdAt의 defaultValue가 생각했던것과 달라요!

MySQL의 경우 defaultValue를 현재 시간(`CURRENT_TIMESTAMP`)로 등록하려 할 때, `Seqeulize.NOW`로 속성을 입력할 경우 정상적으로 반영이 되지 않는 문제가 있습니다. 

해당하는 문제를 해결하기위해 `defaultValue: Sequelize.fn("now")`로 설정하였습니다. [sequelize GH 참고](https://github.com/sequelize/sequelize/issues/4679)

### `sequelize-cli` 사용하지 않고 마이그레이션 하기

cli 말고 일반 패키지 `sequelize`에서도 테이블 생성기능이 존재하다. `sequelize.sync`를 사용하면 되기는 하지만 문제는 테이블을 몽땅 초기화한다는 문제가 있다.

```js
// app.js
const { sequelize } = require("./models/index.js");

async function main() {
  // sequelize에 테이블들이 존재하지 않는 경우 태이블을 생성합니다.
  await sequelize.sync();
}

main();
```

### `Users` table == `User` model

<https://sequelize.org/docs/v6/core-concepts/model-basics/#concept>

> A model in Sequelize has a name. This name does not have to be the same name of the table it represents in the database. Usually, models have singular names (such as User) while tables have pluralized names (such as Users), although this is fully configurable.

마이그레이션 파일에 정의한 테이블의 이름은 복수형으로 쓰고 모델 파일에 정의한 모델의 이름은 단수형으로 써버리는 불상사가 일어났지만 한동안 발각되지 않았다. 심지어 진짜 DB 테이블에 잘 써지기도 해서 도대체 어디서 이 둘을 엮는건지 알 수가 없었다. 공식문서를 통해 알게된 것은, 굳이 모델 이름과 테이블 이름이 일치할 필요가 없다는 것이었다. 그마저도 모델 이름은 단수, 테이블 이름은 복수여도 **fully configurable**하다고... 진짜 이런 우연의 일치가.....

## Relational feature with sequelize

<https://sequelize.org/docs/v6/core-concepts/assocs/>

SQL인데 JOIN을 안할 수가 없겠지? 그래서 foreign key(FK)가 필요하다. 이 FK를 정의하는것이 바로 `references`다. 아래 코드는 migration을 통해 데이터베이스에 스키마를 정의한다. <strong>데이터베이스를 위한 마이그레이션과 sequelize를 위한 모델은 엄연히 다르다!!!</strong>

참고로, FK를 컬럼에 넣는것은 불가능하다. 왜냐하면 FK Constraint에 저촉되기 때문이란다.

```js
// migrations/XXXXXXXX-create-posts.js

'use strict';
/** @type {import('sequelize-cli').Migration} */
module.exports = {
  async up(queryInterface, Sequelize) {
    await queryInterface.createTable('Posts', {
      ...
      UserId: {
        allowNull: false, // NOT NULL
        type: Sequelize.INTEGER,
        references: {
          model: 'Users', // Users 모델을 참조합니다.
          key: 'userId', // Users 모델의 userId를 참조합니다.
        },
        onDelete: 'CASCADE', // 만약 Users 모델의 userId가 삭제되면, Posts 모델의 데이터가 삭제됩니다.
      },
      ...
    });
  },
};
```

___

cardinality를 정의하는 코드는 **model**에서 진행된다.

#### 1:1

- `hasOne` 메서드를 사용하는 모델은 참조컬럼이 생성 ❌
- `belongsTo` 메서드를 사용하는 모델은 참조컬럼이 생성 ⭕️

`Users` 모델은 `UserInfos` 모델을 가지고 있고 (has one), `UserInfos` 모델은 `Users`에게 소유된다 (belongs to). 약타입인 `UserInfo`가 FK를 가지고 있다.

- [?] 약성타입이 FK를 가지고 있어야 하는 이유는?

```js
// models/users.js

'use strict';
const { Model } = require('sequelize');
module.exports = (sequelize, DataTypes) => {
  class Users extends Model {

    static associate(models) {
      // define association here

      // 1. Users 모델에서
      this.hasOne(models.UserInfos, { // 2. UserInfos 모델에게 1:1 관계 설정을 합니다.
        sourceKey: 'userId', // 3. Users 모델의 userId 컬럼을
        foreignKey: 'UserId', // 4. UserInfos 모델의 UserId 컬럼과 연결합니다.
      });
    }
  }
  ...
};
```

```js
// models/userInfos.js

...
// 1. UserInfos 모델에서
this.belongsTo(models.Users, { // 2. Users 모델에게 1:1 관계 설정을 합니다.
  targetKey: 'userId', // 3. Users 모델의 userId 컬럼을
  foreignKey: 'UserId', // 4. UserInfos 모델의 UserId 컬럼과 연결합니다.
});
```

#### 1:N

1 유저는 N개의 포스트를 쓸 수 있어야 한다. 따라서

- `Users`는 `hasMany`를 사용하여야 하고
- `Posts`는 `belongsTo`를 사용하여 FK를 가지게 만들어야 한다.

```js
// models/users.js

// 1. Users 모델에서
this.hasMany(models.Posts, { // 2. Posts 모델에게 1:N 관계 설정을 합니다.
  sourceKey: 'userId', // 3. Users 모델의 userId 컬럼을
  foreignKey: 'UserId', // 4. Posts 모델의 UserId 컬럼과 연결합니다.
});
```

```js
// models/posts.js

// 1. Posts 모델에서
this.belongsTo(models.Users, { // 2. Users 모델에게 N:1 관계 설정을 합니다.
  targetKey: 'userId', // 3. Users 모델의 userId 컬럼을
  foreignKey: 'UserId', // 4. Posts 모델의 UserId 컬럼과 연결합니다.
});
```

#### M:N

[[Many-To-Many relationships {sequelize} {todo}]]

## Lazy / Eager loading and N+1 Problem

- [Fetching associations - Eager Loading vs Lazy Loading](https://sequelize.org/docs/v6/core-concepts/assocs/#fetching-associations---eager-loading-vs-lazy-loading)
- [eager-loading, advanced](https://sequelize.org/docs/v6/advanced-association-concepts/eager-loading)
- [model-querying-finders](https://sequelize.org/docs/v6/core-concepts/model-querying-finders/)

Lazy Loading 예제코드:

자동으로 생성된 `[get|set|create]<Model>[s]` 메서드 (`getPosts`, `getComments`, ...) 들에 관한 자세한 정보는 [Special methods/mixins added to instances](https://sequelize.org/docs/v6/core-concepts/assocs/#special-methodsmixins-added-to-instances)에서 확인바람.

```js
const user = await Users.findOne({
	where { userId }
});

user.nickname;
user.password;
await user.getPosts(); // automatically created method
await user.getComments(); // automatically created method
```

Eager Loading 예제코드:

```js
const user = await Users.findOne({
  attributes: ["userId", "email", "createdAt", "updatedAt"],
  include: [
    {
      model: UserInfos, // Join할 모델 (mandatory)
      attributes: [     // 조회할 컬럼 (optional)
	      "name", "age", "gender", "profileImage"
	  ],
    }
  ],
  where: { userId }
});

user.nickname;
user.password;
user.posts; // 자동으로 복수형 이름을 가진 필드가 생성!
user.comments; // 자동으로 복수형 이름을 가진 필드가 생성!
```

[[Data Modeling {book-project}#N+1 Problem]]을 참조. `Users` ⇄ `UserInfos`를 한 번씩만 조회하기 위해 `include`를 사용한다. Django ORM이 N+1 문제를 어떻게 해결했는지는 기억이 잘 안나지만 여튼 기본동작은 Lazy loading이다. 처음에 모델의 인스턴스를 불러올때 ORM은 연관테이블을 조회하지 않는다. 인스턴스의 연관테이블을 `.` 연산을 통해서 참조하려고 할 때 그제서야 쿼리를 날리게 되고 두 테이블을 JOIN한 뒤 그 튜플의 컬럼을 조사하는 것은 상황에 따라 더 낮은 성능결과를 낳을 수 있다.

만약 인스턴스가 하나의 연관테이블의 컬럼을 N번 참조하게 된다면 JOIN을 N번 수행하게 되고, 즉 똑같은 테이블을 N번 쿼리하게 된다는 것이다. 이 경우 프로그래머는 인스턴스를 쿼리할 때 아예 JOIN을 완료한 커다란 테이블을 로드하는 편이 성능향상에 도움이 될 것이다. 이것을 **Eager Loading**이라고 부른다.

express는 eager loading을 명시적으로 할 수 있는데, finder 함수의 인자로 `include` 속성을 추가하면 된다.

```js
const awesomeCaptain = await Captain.findOne({
  where: {
    name: "Jack Sparrow"
  },
  include: Ship
});
// Now the ship comes with it
console.log('Name:', awesomeCaptain.name);
console.log('Skill Level:', awesomeCaptain.skillLevel);
console.log('Ship Name:', awesomeCaptain.ship.name);
console.log('Amount of Sails:', awesomeCaptain.ship.amountOfSails);
```
