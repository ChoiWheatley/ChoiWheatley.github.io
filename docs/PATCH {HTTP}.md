---
description:
aliases: 
tags: 
created: 2023-06-08T17:05:19
updated: 2023-07-11T15:21:07
title: PATCH {HTTP}
---
- [blog content](https://medium.com/@isuru89/a-better-way-to-implement-http-patch-operation-in-rest-apis-721396ac82bf#68ac)
- [[PUT is idempotent, PATCH isn't]]
- HTTP PATCH 메서드와 함께 사용되는 JSON 규칙이다. PATCH 메서드는 리소스의 일부 필드를 변경하는 용도로 사용한다. 
- RESTful한 PATCH operation들을 공식으로 지원한다. `add, remove, replace, move, copy, test`가 있다. | [RFC 6901 {proposal}](https://datatracker.ietf.org/doc/html/rfc6902)

> Note that, the header`Content-Type` should be set to `application/json-patch+json` media type to indicate that it accepts a set of patch operations, _but it’s not mandatory_. You can have it as `application/json` and still, it will work.

```http
PATCH /users

[  
	{ "op": "test", "path": "/a/b/c", "value": "foo" },  
	{ "op": "remove", "path": "/a/b/c" },  
	{ "op": "add", "path": "/a/b/c", "value": [ "foo", "bar" ] },  
	{ "op": "replace", "path": "/a/b/c", "value": 42 },  
	{ "op": "move", "from": "/a/b/c", "path": "/a/b/d" },  
	{ "op": "copy", "from": "/a/b/d", "path": "/a/b/e" }  
]
```

# RFC 6901

- https://jsonpatch.com
	- **JSON Pointer**: A string format for identifying a specific value within a JSON document. It is used by all operations in JSON Patch to specify the part of the document to operate on. It is specified in RFC 6901 from the IETF.
		- `/` 문자로 구분되어진 문자열로, 객체의 속성에 접근하는 `.` 연산과 완전히 일치한다.
		- 예를 들어 다음과 같은 구조가 있다고 가정하자
		```json
		{
			"biscuits": [
				{"name": "Digestive"},
				{"name": "Choco"},
			]
		}
		```
		그러면 "Digestive"는 `/biscuits/0/name`으로 접근이 가능하고 "Choco"는 `/biscuits/1/name`으로 접근이 가능하여야 한다.
	- `add`
		- 리소스가 배열이나 리스트인 경우, 추가 `value`를 삽입하는 연산이다.
		- `{"op": "add", "path": "biscuits/1", "value": {"name": "Ginger Nut"}}`
	- `remove`
		- 리소스 혹은 리소스 배열의 원소를 삭제하는 연산이다.
		- 리소스 삭제: `{"op": "remove", "path": "/biscuits"}`
		- 리소스 배열의 0번째 원소 삭제: `{"op": "remove", "path": "/biscuits"}`
	- `replace`
		- 리소스의 특정 필드를 수정하는 연산이다.
		- `{"op": "replace", "path": "/biscuits/0/name", "value": "Chocolate"}`
	- `copy`
		- from 리소스를 path 리소스로 복사하는 연산이다. from, path 모두 JSON Pointer이다.
		- `{"op": "copy", "from": "/biscuits/0", "path": "/best_biscuit"}
	- `move`
		- copy와는 달리 from 리소스를 path리소스로 이동한다.
	- `test`
		- 명시한 값이 리소스와 일치하는지 여부를 테스트한다. 테스트를 실패하면 [RFC 6902#Error-Handling](https://datatracker.ietf.org/doc/html/rfc6902#section-5) 와 같이 PATCH 수행을 종료하고 전체 PATCH 연산을 무효화 하여야 한다.
		- PATCH 메서드는 원자적이다. 하나의 PATCH 요청 안에 여러 "op"가 들어있을 수 있으며, 이 중에 하나의 연산이 실패하게 된다면 PATCH 이전 상태로 돌아와야 한다.
		  ```json
		[
			{"op": "replace", "path": "/a/b/c", "value": 42},
			{"op": "test", "path": "/a/b/c", "value": "C"}, // FAILURE because of [0]
		] // Whole operation MUST NOT be patched
		  ```

- 위의 proposal을 누가 이미 구현체로 만들어놓았다. 뭐하는 사람들이지
	- [python-json-patch {GH}](https://github.com/stefankoegl/python-json-patch)

# issue에 대한 응답

아래는 PATCH 요청에 대하여 `jsonpatch` 모듈로 파싱, 유저 프로파일을 수정하는 코드 스니펫이 들어있는 이슈 대화 목록이다.

[bookstores/issues/20](https://github.com/ESTsoft-Book-Project/bookstore/issues/20)

정리하자면,

1. PATCH 하고자 하는 데이터 칼럼의 리스트만 프론트에서 쏴서 `jsonpatch.apply_patch` 에서 수정된 데이터의 `dict`를 받는다.
2. 다만, `User`의 속성이 단순 `dict`가 아니기 때문에 `apply_patch`의 리턴값을 사용하지는 못하고 `for` 문을 돌면서 필드의 값을 일일이 수정하는 모습을 보이고 있다. 
3. 최근 코드를 보니, DRF의 Serializer로 완전히 대체한 모습을 볼 수 있다. PATCH 메서드는 사용했으나 body는 RFC6901를 따르지 않았는데, 의도한 거라고 볼 수 있을 것 같아보인다..? 
4. Serializer에서 제공하는 `partial=True`를 그냥 사용하면 PATCH `"op": "replace"`와 완전히 동등한 코드를 쓸 수 있는데.