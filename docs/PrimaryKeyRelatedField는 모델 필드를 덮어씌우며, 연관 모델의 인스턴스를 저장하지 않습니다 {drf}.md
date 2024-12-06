---
aliases: 
tags: 
description:
title: PrimaryKeyRelatedField는 모델 필드를 덮어씌우며, 연관 모델의 인스턴스를 저장하지 않습니다 {drf}
created: 2024-12-06T23:31:34
updated: 2024-12-06T23:35:26
---
[https://github.com/NohSungwoo/Plana/pull/25/files#diff-f96949d693834953aca5d343f59b82cb607f979dd16891020a6225a084b2b9e8R13](https://github.com/NohSungwoo/Plana/pull/25/files#diff-f96949d693834953aca5d343f59b82cb607f979dd16891020a6225a084b2b9e8R13) 해당 PR의 `memos/serializers.py` 파일에서 의문이 생겼습니다.

> Serializer > Meta > fields 에 포함된 필드는 Model 필드를 덮어씌우는 것이 아닐까? 아래의 Serializer는 `memo_schedule` 이라는 모델 필드 대신에 단순 int 타입 `PrimaryKeyRelatedField`으로 치환할 뿐인가? 아니면 내부적으로 `Schedule` 이라는 연관 모델의 인스턴스를 들고 있을까?

```python
class MemoDetailSerializer(s.ModelSerializer):
    memo_set = s.PrimaryKeyRelatedField(queryset=MemoSet.objects.all())
    memo_schedule = s.PrimaryKeyRelatedField(read_only=True)
    memo_todo = s.PrimaryKeyRelatedField(read_only=True)

    class Meta:
        model = Memo
        fields = (
            "id",
            "title",
            "memo_set",
            "text",
            "memo_schedule",
            "memo_todo",
        )
```

## IF 덮어씌우는 것이 맞다:

Serializer는 한 번 생성된 이후로는 `serializer.instance`의 연관 인스턴스를 저장하지 않는다는 뜻이다. 이 경우 `serializer.instance.memo_schedule.joined_column` 이런 사용이 불가능해진다는 뜻이기도 하다.

## ELSE 덮어씌우지 않고 모델 인스턴스를 가지고 있다:

Serializer가 인스턴스와 연관 인스턴스들을 전부 가지고 있는 경우, 적어도 `update`, `create` 같은 메서드를 오버라이드 할때 편해질 것이다.

## Source Code

[https://github.com/encode/django-rest-framework/blob/3.15.2/rest_framework/relations.py#L249-L250](https://github.com/encode/django-rest-framework/blob/3.15.2/rest_framework/relations.py#L249-L250)

```python

class RelatedField(Field)
		def use_pk_only_optimization(self):
				return False

    def get_attribute(self, instance):
        if self.use_pk_only_optimization() and self.source_attrs:
            # Optimized case, return a mock object only containing the pk attribute.
            with contextlib.suppress(AttributeError):
                attribute_instance = get_attribute(instance, self.source_attrs[:-1])
                value = attribute_instance.serializable_value(self.source_attrs[-1])
                if is_simple_callable(value):
                    # Handle edge case where the relationship `source` argument
                    # points to a `get_relationship()` method on the model.
                    value = value()

                # Handle edge case where relationship `source` argument points
                # to an instance instead of a pk (e.g., a `@property`).
                value = getattr(value, 'pk', value)

                return PKOnlyObject(pk=value)
        # Standard case, return the object instance.
        return super().get_attribute(instance)

```

### PrimaryKeyRelatedField

```python
class PrimaryKeyRelatedField(RelatedField):
    def use_pk_only_optimization(self):
        return True
```

`PrimaryKeyRelatedField`는 `use_pk_only_optimization()`이 True이다. 즉, `RelatedField` 의 `get_attribute` 메서드에서 오직 `.pk` 속성만 가지고 있는 목 객체를 리턴한다고 되어있다. 따라서 연관 필드가 `PrimaryKeyRelatedField` 일 경우, 연관인스턴스는 가지고 있지 않는 것이라고 볼 수 있다.

### HyperlinkedRelatedField

```python
class HyperlinkedRelatedField(RelatedField):
    def use_pk_only_optimization(self):
        return self.lookup_field == 'pk'
        
    def get_object(self, view_name, view_args, view_kwargs):
        """
        Return the object corresponding to a matched URL.

        Takes the matched URL conf arguments, and should return an
        object instance, or raise an `ObjectDoesNotExist` exception.
        """
        lookup_value = view_kwargs[self.lookup_url_kwarg]
        lookup_kwargs = {self.lookup_field: lookup_value}
        queryset = self.get_queryset()

        try:
            return queryset.get(**lookup_kwargs)
        except ValueError:
            exc = ObjectValueError(str(sys.exc_info()[1]))
            raise exc.with_traceback(sys.exc_info()[2])
        except TypeError:
            exc = ObjectTypeError(str(sys.exc_info()[1]))
            raise exc.with_traceback(sys.exc_info()[2])
```

`HyperlinkedRelatedField`는 기본적으로 `use_pk_optimization()`이 True입니다, 굳이 `.lookup_field`를 변경하지 않는 이상. 그리고 최적화가 되어있기 때문에 연관 인스턴스에 대한 내용이 없어 `get_object` 메서드를 통하여 DB에 쿼리를 한 번 더 날려야 하는 것으로 인다. 이 부분은 좀 주의해서 다뤄야 할 것 같다. 별 생각없이 연관 인스턴스 가지고 오다간 수많은 쿼리의 향연을 만나게 될 것이다.

### HyperlinkedIdentityField

```python
class HyperlinkedIdentityField(HyperlinkedRelatedField):
    def use_pk_only_optimization(self):
        # We have the complete object instance already. We don't need
        # to run the 'only get the pk for this relationship' code.
        return False
```

반면에 `HyperlinkedIdentityField`는 `use_pk_only_optimization()` 이 True이다. 그리고 그 주석을 읽어보면…

> 우리는 이미 온전한 객체 인스턴스를 이미 가지고 있습니다. 굳이 “이 관계를 위한 유일한 pk”를 다룰 필요가 없습니다.

이는 목 객체가 필요 없다는 뜻이지, 기본적으로는 `HyperlinkedRelatedField`를 상속받고 있기 때문에 연관 인스턴스의 속성을 조회하기 위해선 `get_object`를 통해야 할 것이다.

---

## 결론: `PrimaryKeyRelatedField`는 모델 필드를 덮어씌우며, 연관 모델의 인스턴스를 저장하지 않습니다.

`PrimaryKeyRelatedField`는 `use_pk_only_optimization()` 메서드가 `True`로 설정되어 있습니다. 이로 인해 DRF의 `get_attribute` 메서드는 최적화된 방식으로 작동하며, 연관된 모델 인스턴스를 참조하지 않고 `PKOnlyObject`를 반환합니다. 따라서, 이 필드를 Serializer에서 사용할 경우 다음과 같은 특징이 있습니다:

1. **연관 인스턴스 비저장**:
    - `memo_schedule` 또는 `memo_todo`와 같은 `PrimaryKeyRelatedField`는 단순히 PK 값을 반환합니다.
    - 결과적으로 `serializer.instance.memo_schedule`로 연관 모델의 속성(예: `joined_column`)에 접근할 수 없습니다.
    - 연관 필드 값이 필요할 경우 추가적으로 쿼리를 실행해야 합니다.
2. **모델 필드 덮어쓰기**:
    - Serializer의 `fields`에 포함된 필드는 모델의 기본 동작을 덮어씌웁니다.
    - `PrimaryKeyRelatedField`는 기본적으로 연관 필드를 단순한 정수형 PK로 표현합니다.
3. **Hyperlinked 필드와의 비교**:
    - `HyperlinkedRelatedField` 또는 `HyperlinkedIdentityField`는 `use_pk_only_optimization()`의 설정에 따라 연관 인스턴스를 사용할 수도 있습니다. 그러나 기본 설정에서는 URL만 반환하며, 실제 객체를 가져오려면 추가적인 DB 조회가 필요합니다.

### 실무적 고려사항:

1. **필요한 필드 설계**:
    - 연관 필드가 단순히 참조(PK)만 필요한 경우 `PrimaryKeyRelatedField`를 사용해 효율적으로 처리할 수 있습니다.
    - 반면, 연관 모델의 속성에 접근해야 한다면 별도의 `SerializerMethodField`를 정의하거나, 모델 필드를 덮어쓰지 않도록 설정해야 합니다.
2. **최적화 고려**:
    - `PrimaryKeyRelatedField`를 사용할 때는 불필요한 DB 조회를 피할 수 있지만, 연관 데이터가 필요할 경우 추가 쿼리를 발생시키므로 주의해야 합니다.
    - N+1 문제를 방지하려면 DRF의 `select_related`나 `prefetch_related`를 적절히 활용하는 것이 중요합니다.
3. **명확한 의사결정**:
    - 특정 필드가 연관 모델의 속성이나 데이터를 포함해야 한다면 `Serializer`에서 명시적으로 정의하고 필요한 데이터를 포함하는 것이 좋습니다. 예를 들어, `memo_schedule` 필드에 대한 상세 정보를 제공하려면 `Nested Serializer`를 사용하는 방법도 고려할 수 있습니다.

위 결론을 바탕으로 PR의 Serializer가 연관 인스턴스를 저장하지 않는다는 점을 염두에 두고, 연관 모델의 속성이 필요한 경우 대체 설계를 고려하세요.
