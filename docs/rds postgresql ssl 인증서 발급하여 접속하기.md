---
aliases: 
tags: 
description:
title: rds postgresql ssl 인증서 발급하여 접속하기
created: 2024-03-10T23:19:32
updated: 2024-03-10T23:22:27
---

## Error 상황

[[0018.1 Nest.js 🐱]] 에서 RDS에 연결하려다 다음 오류 메시지를 확인하게 되었습니다.

``` 
 no pg_hba.conf entry for host "***.***.***.***", user "postgres", database "postgres", no encryption
```

## 원인

Amazon RDS PostgreSQL 인스턴스는 SSL / TLS를 이용하여 클라이언트와 연결하는 것이 강제돼 있기 때문에 AWS측에서 제공하는 `.pem`키를 받아다 써야 한다.

## 해결

<https://velog.io/@bshunter/AWS-ec2-rds-no-pghba.conf-entry-%EC%97%90%EB%9F%AC> 블로그에 따르면

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
