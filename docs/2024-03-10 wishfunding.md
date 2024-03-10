---
aliases: 
tags: 
description:
title: 2024-03-10 wishfunding
created: 2024-03-10T13:34:36
updated: 2024-03-10T23:18:28
---

## timestamp 칼럼을 어떻게 표현하더라?

- <https://jojoldu.tistory.com/600>

내장 `Date` 객체를 사용하기엔 불변타입이 아니라서 에러를 자주 일으킬 가능성이 높다고 한다. 그래서 [js-joda](https://www.npmjs.com/package/js-joda)를 제안하고 있는 블로그이다.

- [[DateColumn Decorators {typeorm}]] 를 사용하면 row 생성, 수정, 삭제에 대한 시간을 자동으로 관리할 수 있어 편리하다.

> 하지만 나는 임의의 Date를 저장해야 할 필요가 있는데요?

`endAt` 이라는 칼럼은 펀딩이 끝나는 시간을 저장한다. 그러므로 `DateColumn` 데코레이터만으로는 처리할 수 없게된다.

```ts
   /**
	 * https://typeorm.io/entities#column-types
	 *
     * TODO - timestamptz를 사용할지, 일반 date를 사용할지 결정해야함.
     * timestamptz는 시간 & 타임존을 포함하고 date는 말 그대로 날짜만 저장함
     */
    @Column("date")
    // @Column("timestamptz")
    endAt: Date;
```

## ✌️ Typeorm Column Validation

save를 할 때 금액칸에 음수가 들어갈 경우 Typeorm에서 잘못된 값이 들어갔는지 여부를 판단하여 에러를 발생하게 만들어 줄 수 있다면 좋을 것 같다.

[[Validation {typeorm}]]

## 칼럼 이름을 내가 직접 정의할 수는 없을까?

`OneToMany`, `ManyToOne`으로 프로퍼티들을 만들다보면 ER 다이어그램을 작성하며 열심히 정의했던 FK들이랑 이름이 달라지는 경우가 발생하게 되었다. 그래서 ORM이 자동으로 FK를 만들어 줄 때 그 이름을 내가 먹일 수는 없는걸까?

## Uuid가 필요한 테이블

- `Funding`

혹시 더 있나? `Funding` 테이블에 `fundUuid` 프로퍼티 추가해야함.

## DTO와 Typeorm

Data Transfer Object로, 데이터를 전달하기 위해 있는 객체임. DTO 객체의 클래스를 정의하면 컨트롤러에서 타입을 자동으로 DTO로 변환해서 받아볼 수 있다. recre-backend에서의 예시를 다시 복습해보자:

- `user.controller.ts`

```ts
export class UserController {
	...
    @Post()
    create(@Body() createUserDto: CreateUserDto) {
        return this.userService.createUser(createUserDto);
    }
}
```

- `user.service.ts`

```ts
export class UserService {
	...
    createUser(createUserDto: CreateUserDto): Promise<User> {
        const user: User = new User();
        user.nickname = createUserDto.nickname;
        user.profileImage = createUserDto.profileImage;
        user.provider = createUserDto.provider;
        user.email = createUserDto.email;
        user.createdDt = new Date(Date.now());
        return this.userRepository.save(user);
    }
}
```

DTO에 요청시 프로퍼티들의 타입에 제한을 걸 수 있고, -1원과 같이 말이 안되는 경우도 [[Validation {typeorm}]]을 통해 걸러낼 수 있다고 한다.

[[DTO Validation using class-validator {NestJS}]]

## 회의 안건..

- [x] 현진이 github 계정 ChoiWheatley / wishlist_funding_dbdiagram 리포지토리에 collaborator 권한 부여
- [ ] uuid 필요한 테이블 식별

## 회의 진행중

### no pg_hba.conf entry 에러

<https://velog.io/@bshunter/AWS-ec2-rds-no-pghba.conf-entry-%EC%97%90%EB%9F%AC>

```
 no pg_hba.conf entry for host "***.***.***.***", user "postgres", database "postgres", no encryption
```

먼저 해결 방법은 다음과 같습니다.

1. 다음 링크를 사용하여 모든 AWS 리전에 대한 인증서 번들을 다운로드하세요: [https://truststore.pki.rds.amazonaws.com/global/global-bundle.pem](https://truststore.pki.rds.amazonaws.com/global/global-bundle.pem)
    
2. DB연결 시 해당 인증서를 함께 전송하세요
    

다음은 typeOrm을 이용한 예시 코드입니다.

해당 코드를 추가해 주면 해결됩니다.

```typescript
ssl: {
    // 다운로드한 인증서 파일 경로 추가
    ca: fs.readFileSync('your-path-to/global-bundle.pem')
  },
  extra: {
    // SSL 연결을 강제 설정
    ssl: { rejectUnauthorized: false },
  },
```

전체 코드 예시

```typescript
import { TypeOrmModuleOptions } from '@nestjs/typeorm';
import * as config from 'config';
import * as fs from 'fs';

const dbconfig = config.get('db');
require('dotenv').config();

export const typeORMConfig: TypeOrmModuleOptions = {
  type: dbconfig.type,
  host: process.env.RDS_HOSTNAME || dbconfig.host,
  port: process.env.RDS_PORT || dbconfig.port,
  username: process.env.RDS_USERNAME || dbconfig.username,
  password: process.env.RDS_PASSWORD || dbconfig.password,
  database: process.env.RDS_DB_NAME || dbconfig.database,
  entities: [__dirname + '/../**/*.entity.{js,ts}'],
  synchronize: true,
  logging: true,
  ssl: {
    // 다운로드한 인증서 파일 경로 추가
    ca: fs.readFileSync('your-path-to/global-bundle.pem')
  },
  extra: {
    // SSL 연결을 강제 설정
    ssl: { rejectUnauthorized: false },
  },
  autoLoadEntities: dbconfig.synchronize,
};
```

해당 오류가 나는 이유는 Amazon RDS PostgreSQL 인스턴스는 기본적으로 SSL/TLS를 사용하여 클라이언트와 연결하도록 설정되어 있기 때문입니다.

공식 문서에 따르면,  
"사용자는 SSL 연결에 인증서를 사용하고, "sslmode=verify-ca" 또는 "sslmode=verify-full"로 설정하여 클라이언트가 인증서 체인을 확인하도록 해야 합니다. "  
라고 나와있습니다.

에러 해결에 도움이 되었길 바랍니다.

공식문서:[https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.SSL.html](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.SSL.html)
