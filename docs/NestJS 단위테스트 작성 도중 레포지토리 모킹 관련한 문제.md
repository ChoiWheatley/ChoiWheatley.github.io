---
aliases: 
tags: 
description:
title: NestJS 단위테스트 작성 도중 레포지토리 모킹 관련한 문제
created: 2024-12-15T16:47:49
updated: 2024-12-15T16:59:22
---
[nest/issues # Unable to run tests because Nest can't resolve dependencies of a service](https://github.com/nestjs/nest/issues/363)

상위 이슈 대화목록에서는 Repository 모킹에 관한 토론이 이어졌다. 가장 많은 좋아요를 받은 게시글은 `<entity-name>Repository` 을 직접 주입하는 방법이 문제를 해결했다고 한다. 그 방법으로, typeorm의 `Repository<Entity>`를 상속하여 테스트 모듈의 DI 컨테이너에 등록하라고 한다.

일단 `<entity-type>Repository`를 일일이 만드는 것은 우발적 복잡성을 유발할 가능성이 있기 때문에 피해야 할 것으로 보인다. 일단 DI 컨테이너의 providers 프로퍼티에 `{provide: getRepositoryToken(Entity), useClass: Repository}`를 사용해서 TypeORM 모듈 대신 레포지토리를 모킹할 수 있는지 확인해보자.
