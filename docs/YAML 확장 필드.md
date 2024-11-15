---
aliases: 
tags: 
description:
title: YAML 확장 필드
created: 2024-11-15T17:05:18
updated: 2024-11-15T17:05:20
---

YAML(또는 'YAML Ain't Markup Language')은 사람이 읽기 쉬운 데이터 직렬화 형식으로, 구성 파일 작성 등에 널리 사용됩니다. YAML은 반복적인 구성을 줄이고 파일을 모듈화하기 위해 '확장 필드'와 '앵커 및 별칭' 기능을 제공합니다.

## 확장 필드

확장 필드는 `x-`로 시작하는 최상위 수준의 YAML 노드로, 주로 공통 구성을 정의하고 이를 재사용하는 데 사용됩니다. 이러한 필드는 YAML 파서에 의해 무시되므로, 실제 데이터에는 포함되지 않습니다.

**예시:**

```yaml
x-common-env: &common-env
  environment:
    - CONFIG_KEY
    - EXAMPLE_KEY

services:
  service1:
    <<: *common-env
    image: my-image:latest
  service2:
    <<: *common-env
    image: another-image:latest
```

위 예시에서 `x-common-env` 확장 필드는 공통 환경 변수를 정의하고, `service1`과 `service2`에서 이를 참조하여 중복을 줄였습니다.

## 앵커 및 별칭

YAML에서는 `&`를 사용하여 앵커를 정의하고, `*`를 사용하여 해당 앵커를 참조하는 별칭을 생성할 수 있습니다. 이를 통해 중복된 구성을 피하고 유지보수를 용이하게 할 수 있습니다.

**예시:**

```yaml
default-settings: &defaults
  timeout: 30
  retries: 3

service1:
  <<: *defaults
  timeout: 60  # 이 서비스는 기본 설정을 재정의합니다.

service2:
  <<: *defaults
```

여기서 `&defaults`로 앵커를 정의하고, `<<: *defaults`를 통해 이를 참조하여 `service1`과 `service2`에 기본 설정을 적용했습니다. `service1`은 `timeout` 값을 재정의하여 특정 설정을 적용했습니다.

## 활용 사례

- **Docker Compose 파일 모듈화:** 여러 서비스에서 공통으로 사용하는 설정을 확장 필드와 앵커를 통해 정의하여 구성 파일의 중복을 줄이고 유지보수를 용이하게 할 수 있습니다.

- **복잡한 구성 관리:** 대규모 시스템에서 공통 설정을 중앙에서 관리하고, 필요한 부분에서만 재정의하여 일관성을 유지할 수 있습니다.

이러한 기능을 활용하면 YAML 파일의 가독성과 유지보수성을 크게 향상시킬 수 있습니다. 
