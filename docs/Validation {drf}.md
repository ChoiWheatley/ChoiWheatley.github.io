---
aliases: 
tags: 
description:
title: Validation {drf}
created: 2024-11-26T01:14:15
updated: 2024-11-26T01:16:14
---

- ref: <https://www.django-rest-framework.org/api-guide/serializers/#validation>

> GPT 응답

# Django REST Framework Serializer Validation

Django REST Framework (DRF)의 Serializer는 데이터 직렬화 및 역직렬화 과정을 처리하며, 입력 데이터를 검증하는 강력한 메커니즘을 제공합니다. 이 문서에서는 DRF Serializer의 Validation에 대해 설명하고, 유효성 검사를 수행하는 방법과 주요 메서드에 대해 다룹니다.

---

## Validation의 주요 개념

Serializer의 Validation은 입력 데이터를 처리하기 전에 데이터가 유효한지 확인하는 단계입니다. Validation은 일반적으로 다음과 같은 세 가지 수준에서 수행됩니다.

1. **필드 수준 검증**: 각 개별 필드에 대해 정의된 조건을 검증합니다.
2. **객체 수준 검증**: 입력된 데이터 전체에 대해 추가적인 검증 로직을 실행합니다.
3. **정의된 제약 조건 검증**: 모델 또는 특정 요구사항에 따라 자동으로 적용되는 검증입니다.

---

## Validation 구현 방법

### 1. 필드 수준 검증

필드 수준 검증은 Serializer의 `validate_<field_name>` 메서드를 사용하여 특정 필드에 대한 유효성 검사를 정의합니다.

```python
from rest_framework import serializers

class ExampleSerializer(serializers.Serializer):
    name = serializers.CharField(max_length=100)
    age = serializers.IntegerField()

    def validate_age(self, value):
        if value < 0:
            raise serializers.ValidationError("Age must be a positive number.")
        return value
```

**동작 원리**:
- `validate_<field_name>` 메서드는 해당 필드의 값에 대해 호출됩니다.
- 유효하지 않은 값에 대해 `serializers.ValidationError`를 발생시킵니다.

---

### 2. 객체 수준 검증

객체 수준 검증은 Serializer의 `validate` 메서드를 사용하여 여러 필드를 조합한 검증을 수행합니다.

```python
class ExampleSerializer(serializers.Serializer):
    start_date = serializers.DateField()
    end_date = serializers.DateField()

    def validate(self, data):
        if data['start_date'] > data['end_date']:
            raise serializers.ValidationError("Start date must be before end date.")
        return data
```

**동작 원리**:
- `validate` 메서드는 모든 필드 데이터를 포함하는 딕셔너리를 입력으로 받습니다.
- 여러 필드 간의 상호 의존성을 검증할 때 유용합니다.

---

### 3. `validators` 옵션

Serializer 필드에 `validators` 인자를 사용하여 사용자 정의 검증기를 추가할 수 있습니다.

```python
from rest_framework.validators import ValidationError

def multiple_of_ten(value):
    if value % 10 != 0:
        raise ValidationError("This field must be a multiple of ten.")

class ExampleSerializer(serializers.Serializer):
    score = serializers.IntegerField(validators=[multiple_of_ten])
```

---

## ValidationError

유효성 검사 실패 시 `serializers.ValidationError` 예외가 발생합니다. 이 예외는 일반적으로 클라이언트로 반환되는 응답에 포함될 수 있는 직렬화된 에러 메시지를 제공합니다.

예:

```python
{
    "age": ["Age must be a positive number."],
    "non_field_errors": ["Start date must be before end date."]
}
```

---

## 참고 사항

- DRF Serializer는 Django 모델 필드의 기본 검증 로직을 자동으로 상속받습니다.
- 데이터 입력 시 적절한 에러 메시지를 사용자에게 제공하여 유효성 검증 실패의 이유를 명확히 전달하는 것이 중요합니다.
- 복잡한 검증 로직은 객체 수준 검증으로 구현하는 것이 유지보수에 유리합니다.

---

### 예제: 전체적인 Validation 활용

```python
class UserSerializer(serializers.Serializer):
    username = serializers.CharField(max_length=150)
    email = serializers.EmailField()
    password = serializers.CharField(write_only=True)

    def validate_username(self, value):
        if "admin" in value.lower():
            raise serializers.ValidationError("Username cannot contain 'admin'.")
        return value

    def validate(self, data):
        if len(data['password']) < 8:
            raise serializers.ValidationError({"password": "Password must be at least 8 characters long."})
        return data
```

---

## 공식 문서

더 자세한 내용은 DRF 공식 문서를 참조하세요: [Django REST Framework Serializer Validation](https://www.django-rest-framework.org/api-guide/serializers/#validation)
