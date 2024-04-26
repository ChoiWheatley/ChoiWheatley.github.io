---
aliases: 
tags: 
description:
title: flask smorest
created: 2024-04-25T23:24:03
updated: 2024-04-26T15:33:27
---

# 04. flask-smorest 활용하여 REST API 생성

공식문서: <https://flask-smorest.readthedocs.io/en/latest/index.html>

OpenAPI 명세를 자동으로 만들어주는 고마운 라이브러리. 키워드: **Bluprint**, **MethodView**, **Schema**

## Simple Example (공식문서)

1. instantiate an Api with Flask application

    ```python
    app = Flask(__name__)
    app.config["API_TITLE"] = "MY API"
    app.config["API_VERSION"] = "v1"
    app.config["OPENAPI_VERSION"] = "3.0.2"
    api = Api(app)
    ```

2. marshmallow **Schema**를 정의하면 리소스의 모델을 정의할 수 있다. 아예 `ma.fields.<type>` 이렇게 스키마의 타입까지 쓰고있네!

    ```python
    class PetSchema(ma.Schema):
        id = ma.fields.Int(dump_only=True)
        name = ma.fields.String()
    ```

3. masrhmallow **Schema**를 정의하면 HTTP 쿼리 인자들을 정의할 수 있다. 이건 확실히 DTO 같네.

    ```python
    class PetQueryArgsSchema(ma.Schema):
        name = ma.fields.String()
    ```

4. **Blueprint**를 생성하자.

    ```python
    blp = Blueprint("pets", "pets", url_prefix="/pets", description="Operations on pets")
    ```

5. **MethodView** 클래스를 상속하여 리소스(path)를 정의하자. 그리고 각각의 메서드에 `blp` 데코레이터를 추가하여 어떤 파라메터 (쿼리, 바디)인지 명시할 수 있다(`Blueprint.arguments`). 그리고 HTTP 응답의 형식을 정의할 수 있다(`Blueprint.response`).

    ```python
    @blp.route("/") # path
    class Pets(MethodView):
        @blp.arguments(PetQueryArgsSchema, location="query") # query param으로 요청을 받겠다.
        @blp.response(200, PetSchema(many=True)) # 응답으로 PetSchema 타입을 주겠다. 그런데 리스트로 주겠다.
        def get(self, args): # HTTP METHOD
            return Pet.get(filters=args) # Service (세부 로직)
        
        @blp.arguments(PetSchema)
        @blp.response(201, PetSchema)
        def post(self, new_data): # new_data는 body에 담겨오겠지?
            item = Pet.create(**new_data)
            return item
    ```

## MethodView 더 보기

`Blueprint.arguments`는 역직렬화를 수행하는 데코레이터이다. 요청 결과를 view function에 주입하여 인자로 담아준다고 한다. [flask-smorest / Arguments](https://flask-smorest.readthedocs.io/en/latest/index.html)

- arguments location
  - "json"
  - "query"
  - "path"
  - "form"
  - "headers"
  - "cookies"
  - "files"
  - "json_or_form"

`Blueprint.arguments`는 하나의 메서드에 여러번 호출될 수 있다. 그래서 query 파라메터와 body 둘 다 필요한 경우, 아래와 같이 쓸 수 있다:

```python
@blp.route("/")
class Pets(MethodView):
    @blp.arguments(PetSchema)
    @blp.arguments(QueryArgsSchema, location="query")
    def post(pet_data, query_args):
        return Pet.create(pet_data, **query_args)
```

## abort

## OpenAPI 

<https://flask-smorest.readthedocs.io/en/latest/openapi.html>
