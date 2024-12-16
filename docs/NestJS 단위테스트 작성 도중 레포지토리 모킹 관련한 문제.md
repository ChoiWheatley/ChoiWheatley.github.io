---
aliases: 
tags: 
description:
title: NestJS 단위테스트 작성 도중 레포지토리 모킹 관련한 문제
created: 2024-12-15T16:47:49
updated: 2024-12-17T01:33:46
---
[nest/issues # Unable to run tests because Nest can't resolve dependencies of a service](https://github.com/nestjs/nest/issues/363)

상위 이슈 대화목록에서는 Repository 모킹에 관한 토론이 이어졌다. 가장 많은 좋아요를 받은 게시글은 `<entity-name>Repository` 을 직접 주입하는 방법이 문제를 해결했다고 한다. 그 방법으로, typeorm의 `Repository<Entity>`를 상속하여 테스트 모듈의 DI 컨테이너에 등록하라고 한다.

일단 `<entity-type>Repository`를 일일이 만드는 것은 우발적 복잡성을 유발할 가능성이 있기 때문에 피해야 할 것으로 보인다. 일단 DI 컨테이너의 providers 프로퍼티에 `{provide: getRepositoryToken(Entity), useClass: Repository}`를 사용해서 TypeORM 모듈 대신 레포지토리를 모킹할 수 있는지 확인해보자.

2024-12-17 update: 결국은 해냈다 🎉 아래 두가지 사용사례(DB 모킹, TypeOrmModule 의존)에 대한 방법을 모두 작성해놓았다.

---

# NestJS 테스트 코드 작성 가이드

NestJS에서 테스트 코드를 작성할 때, 데이터베이스 접근 여부에 따라 테스트 환경을 설정하는 방법이 다릅니다. 이 문서에서는 두 가지 경우에 대한 설정 방법과 예제를 설명합니다.

---

## 1. TypeORM 레포지토리 접근이 필요한 경우

데이터베이스에 실제 쿼리를 요청해야 하는 경우, TypeORM의 `TypeOrmModule`을 테스트 모듈에 추가해야 합니다. 이 경우 RDS 테스트 데이터베이스에 접근하도록 설정합니다.

### 설정 방법

1. **`data-source-options.ts` 정의**
    
    `dataSourceOptions`를 정의하여 데이터베이스 연결 정보를 설정합니다. 또한, 테스트에서 사용할 엔티티 목록을 포함하도록 `createDataSourceOptions` 함수를 사용합니다.

    ```tsx
    export const dataSourceOptions: DataSourceOptions = {
      type: 'postgres',
      host: process.env.DB_HOST,
      port: parseInt(process.env.DB_PORT) || 5432,
      password: process.env.DB_TEST_PASSWORD,
      username: process.env.DB_TEST_USERNAME,
      database: process.env.DB_TEST_DATABASE,
      synchronize: true,
      logging: false,
      dropSchema: true,
      ssl: {
        ca: readFileSync('global-bundle.pem'),
      },
      extra: {
        ssl: {
          rejectUnauthorized: false,
        },
      },
    };
    
    export function createDataSourceOptions(
      entities: EntityClassOrSchema[],
    ): DataSourceOptions {
      return { ...dataSourceOptions, entities };
    }
    
    ```

2. **테스트 모듈 설정**
    
    테스트에서 사용할 엔티티와 `TypeOrmModule`을 설정합니다.

    ```tsx
    beforeEach(async () => {
      const module: TestingModule = await Test.createTestingModule({
        imports: [
          EventEmitterModule.forRoot(),
          TypeOrmModule.forRoot(createDataSourceOptions(entities)),
          TypeOrmModule.forFeature(entities),
        ],
        providers: [
          GiftogetherExceptions,
          MatchDepositUseCase,
          FindProvDonationsBySenderSigUseCase,
        ],
      }).compile();
    
      // 테스트 코드에서 필요한 Repository 주입
      donationRepository = module.get<Repository<ProvisionalDonation>>(
        getRepositoryToken(ProvisionalDonation),
      );
    });
    
    ```

---

## 2. MockRepository로 호출 여부만 테스트하는 경우

데이터베이스에 실제로 접근할 필요가 없는 경우, 레포지토리 메서드 호출 여부만을 테스트합니다. 이런 경우 `TypeOrmModule`을 설정하지 않고 Mock 객체를 활용하여 테스트를 진행합니다.

### 설정 방법

1. **MockRepository 및 MockProvider 활용**
    
    아래 `createMockRepository`와 `createMockProvider` 헬퍼 함수는 이미 구현되어 있으므로, 여러분이 직접 작성할 필요는 없습니다. 이 함수는 모든 레포지토리 메서드와 `createQueryBuilder` 메서드를 포함한 모킹을 제공합니다.

	```typescript
	export function createMock<T>(cls: new (...args: any[]) => T): jest.Mocked<T> {
	  const mock: Partial<jest.Mocked<T>> = {};
	
	  Object.entries(Object.getOwnPropertyDescriptors(cls.prototype)).forEach(
	    ([key, descriptor]) => {
	      if (typeof descriptor.value === 'function' && key !== 'constructor') {
	        mock[key] = jest.fn();
	      }
	    },
	  );
	
	  return mock as jest.Mocked<T>;
	}
	
	type MockRepository<T> = jest.Mocked<T> & {
	  createQueryBuilder: jest.Mocked<SelectQueryBuilder<T>>;
	};
	
	function createMockSelectQueryBuilder<T>(): jest.Mocked<SelectQueryBuilder<T>> {
	  return {
	    select: jest.fn().mockReturnThis(),
	    addSelect: jest.fn().mockReturnThis(),
	    where: jest.fn().mockReturnThis(),
	    update: jest.fn().mockReturnThis(),
	    andWhere: jest.fn().mockReturnThis(),
	    orWhere: jest.fn().mockReturnThis(),
	    set: jest.fn().mockReturnThis(),
	    setParameter: jest.fn().mockReturnThis(),
	    getMany: jest.fn().mockResolvedValue([]), // Example of mocked result
	    getOne: jest.fn().mockResolvedValue(null),
	    execute: jest.fn().mockResolvedValue({}),
	    // Include other methods as needed
	  } as unknown as jest.Mocked<SelectQueryBuilder<T>>;
	}
    /**
     * 아래 함수는 Repository<Entity>를 모킹하기 위해
     * 사용됩니다. 즉, 단위테스트를 위해 실제 RDS에 데이터를 저장하는
     * 것이 아닌, MockRepository에 호출만 합니다.
     *
     * 만약 실제로 데이터를 넣고 그 결과를 재가공해야 할 필요가 있다면
     * 아래 두 가지 방법 중 하나를 사용할 수 있습니다:
     *
     * 1. TypeOrmModule을 `imports:`에 추가한다. 테스트 DB에 직접
     *    데이터를 CRUD한다. (src/tests/data-source-options.ts 참조)
     * 2. 사용하고자 하는 메서드만 따로 구현한 MockRepository를 작성하여
     *    `useValue:` 자리에 할당한다.
     */
    export function createMockRepository<T>(
      cls: new (...args: any[]) => T,
    ): MockRepository<T> {
      const mock = createMock(cls) as MockRepository<T>;
      mock.createQueryBuilder = jest.fn(createMockSelectQueryBuilder) as any;
    
      return mock;
    }
    
    export const createMockProvider = (entity: EntityClassOrSchema): Provider => ({
      provide: getRepositoryToken(entity),
      useValue: createMockRepository(Repository<typeof entity>),
    });
    
    ```

2. **테스트 모듈 설정**
    
    MockRepository를 `providers`에 등록하여 테스트를 진행합니다.

    ```tsx
    beforeEach(async () => {
      const module: TestingModule = await Test.createTestingModule({
        providers: [
          DepositEventHandler,
          NotificationService,
          GiftogetherExceptions,
          CreateDonationUseCase,
          IncreaseFundSumUseCase,
          GetDonationsByFundingUseCase,
          ...entities.map(createMockProvider),
        ],
      }).compile();
    });
    
    ```

---

## 결론

- **TypeORM 레포지토리 접근이 필요한 경우**: `TypeOrmModule`을 설정하고 실제 데이터베이스를 사용합니다.
- **MockRepository로 충분한 경우**: `createMockRepository`와 `createMockProvider`를 활용하여 유닛 테스트를 진행합니다.
