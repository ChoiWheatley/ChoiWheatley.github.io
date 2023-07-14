---
description:
aliases: 
tags: 
created: 2023-05-09T16:51:13
updated: 2023-07-12T10:47:49
title: json-generator
---

{% raw %}

- https://json-generator.com/
- 무작위 JSON을 얼마든지 뽑아낼 수 있게 만들어주는 사이트이다. 완전 개쩐다.
- 예를 들어서 아래 코드는 모의 회원 데이터를 생성해준다.
```JSON
[
  '{{repeat(5, 7)}}',
  {
    _id: '{{objectId()}}',
    age: '{{integer(20, 40)}}',
    eyeColor: '{{random("blue", "brown", "green")}}',
    name: '{{firstName()}} {{surname()}}',
    gender: '{{gender()}}'
  }
]
```

output:
```json
[
  {
    "_id": "6019fd4354979ca26b8f91dc",
    "age": 26,
    "eyeColor": "green",
    "name": "Bender Allen",
    "gender": "male"
  },
  {
    "_id": "6019fd43db2951868889a0b4",
    "age": 27,
    "eyeColor": "blue",
    "name": "Jacobs Golden",
    "gender": "male"
  },
  {
    "_id": "6019fd438c0bf8e775c28536",
    "age": 27,
    "eyeColor": "brown",
    "name": "Grimes Oneal",
    "gender": "male"
  },
  {
    "_id": "6019fd43b68a7fd8b081ab26",
    "age": 40,
    "eyeColor": "blue",
    "name": "Melissa Joyce",
    "gender": "female"
  },
  {
    "_id": "6019fd436c1edc4757aabb9d",
    "age": 32,
    "eyeColor": "blue",
    "name": "Malone Bush",
    "gender": "male"
  },
  {
    "_id": "6019fd438eaf753918f55226",
    "age": 34,
    "eyeColor": "green",
    "name": "Davenport Hyde",
    "gender": "male"
  },
  {
    "_id": "6019fd43c35929ff95004109",
    "age": 20,
    "eyeColor": "blue",
    "name": "Shauna Blevins",
    "gender": "female"
  }
]
```

{% endraw %}
