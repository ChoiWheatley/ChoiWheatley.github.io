---
aliases: 
tags: 
description:
created: 2023-06-19T13:39:35
updated: 2023-07-15T21:33:04
title: name이 유일하지 않을 때 slug 중복을 피하는 법 {django}
---

# Question

  
안녕하세요! 에러 처리의 일환으로 팀원들이 겪은(겪을) 다양한 에러에 대하여 조사를 진행하고 있습니다. 2023-06-17 진행한 회의에서 book name의 unique를 해제할 시 발생할 slug(handle)의 중복문제를 해결하는 과정에 대하여 해당 스레드의 답글로 달아주시면 감사하겠습니다!

따봉따봉 👍

# Answer by Allen9535

해당 사항에 대해 두 가지 해결책을 생각해 봤습니다.  
첫 번째는 상품에 대한 슬러그를 생성할 때, 상품의 PK를 함께 넣어주는 방식입니다. 현재 모델에는 따로 PK가 설정되어 있지 않아서 그런지 생성된 순서대로 숫자를 매겨 PK로 사용하고 있는 것 같습니다. 한 번 생성된 상품을 삭제하더라도 해당 PK는 결번이 되고요. 그래서 이름과 함께 PK를 사용하면 유니크한 슬러그가 될 것 같습니다.

```python
request_data["handle"] = slugify(f"{request_data['name']}-{request_data['user']}")
```

해당 코드로 생성된 슬러그: teseuteucaeg17-15

두 번째는 uuid를 활용하는 것입니다. 슬러그의 길이가 길어진다는 단점(16자리)은 있지만, 나름의 규칙에 의해 유니크한 슬러그를 만들 수 있다는 장점이 있다고 생각합니다. 파이썬에서 기본 제공하는 uuid 모듈에는 uuid1(), uuid3(), uuid4(), uuid5() 라는 네 가지 함수가 있고, 각각 다른 방식으로 uuid를 생성해 준다고 합니다. | [wiki.org](https://en.wikipedia.org/wiki/Universally_unique_identifier) | [docs.python.org](https://docs.python.org/3/library/uuid.html)

```python
request_data["handle"] = slugify(f"{uuid.uuid5(uuid.NAMESPACE_URL, request_data['name'])}")
```

해당 코드로 생성된 슬러그: daadf289-29b0-5faf-946e-8ca97947c50e

- 파이썬에서 제공하는 uuid 모듈의 함수 종류
    - uuid1(): 현재 시간과 MAC 주소 기반
    - uuid3(namespace, name): 네임스페이스와 이름을 기반으로 MD5 해시 함수 사용
    - uuid4(): 무작위 숫자
    - uuid5(namespace, name): 네임스페이스와 이름을 기반으로 SHA-1 해시 함수 사용
