---
aliases: 
tags: 
description:
title: save both relations {typeorm}
created: 2024-04-14T22:58:05
updated: 2024-12-18T18:25:37
---

`Photo` 엔티티와 관련된 `PhotoMetadata` 엔티티를 저장하면서 두 엔티티 간의 관계를 올바르게 설정하는 방법입니다.

### 단계:

1. **엔티티 인스턴스화:**
   - 먼저 `Photo`와 `PhotoMetadata` 엔티티의 인스턴스를 생성합니다:

     ```ts
     const photo = new Photo();
     const metadata = new PhotoMetadata();
     ```

   - 이 엔티티들은 데이터베이스에 저장될 데이터를 나타냅니다.

2. **레포지토리 가져오기:**
   - 각각의 엔티티에 대해 데이터베이스 작업을 처리하는 레포지토리를 가져옵니다:

     ```ts
     const photoRepository = AppDataSource.getRepository(Photo);
     const metadataRepository = AppDataSource.getRepository(PhotoMetadata);
     ```

3. **`Photo` 엔티티를 먼저 저장:**
   - **순서가 중요한 이유:** `PhotoMetadata` 엔티티는 관계를 통해 `Photo`를 참조할 수 있습니다. 따라서, `Photo`를 먼저 저장하면 데이터베이스에서 유효한 ID(예: 기본 키)가 생성되며, 이를 통해 `PhotoMetadata`가 올바르게 참조할 수 있습니다.
   - `photo` 엔티티를 데이터베이스에 저장합니다:

     ```ts
     await photoRepository.save(photo);
     ```

4. **그 다음 `PhotoMetadata` 엔티티 저장:**
   - `photo`가 성공적으로 저장된 후, 이제 `PhotoMetadata` 엔티티를 저장합니다:

     ```ts
     await metadataRepository.save(metadata);
     ```

   - 이 시점에서 `Photo`와 `PhotoMetadata` 간의 관계를 저장된 `photo` 인스턴스를 사용해 완전히 설정할 수 있습니다.

---

**순서의 중요성:**
- **엔티티 간의 의존성:** `PhotoMetadata` 엔티티는 종종 `Photo` 엔티티에 의존하여 관계를 설정합니다. 예를 들어, `PhotoMetadata`가 `Photo`를 참조하는 외래 키를 가지고 있다면, 관계를 저장하기 전에 `Photo`가 데이터베이스에 존재해야 합니다.
- **잠재적 오류:** 이 순서를 반대로 하면 외래 키 제약 위반 또는 `PhotoMetadata` 엔티티에서 null 참조와 같은 문제가 발생할 수 있습니다.

위와 같은 순서를 따름으로써, `PhotoMetadata` 엔티티가 저장된 `Photo` 엔티티를 올바르게 참조할 수 있도록 보장할 수 있습니다.
