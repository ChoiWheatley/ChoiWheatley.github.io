---
aliases: 
tags: 
description:
title: psychological_test_completion 분석.flask
created: 2024-05-05T19:44:09
updated: 2024-05-07T11:24:59
---
[github.com / ChoiWheatley / psychological_test_completion](https://github.com/ChoiWheatley/psychological_test_completion)

## 파일 구조

```
.
├── app --> 모듈 이름
│   ├── __init__.py ---------> 모듈 엔트리 포인트
│   ├── database.py ---------> db 객체 생성 (SQLAlchemy)
│   ├── models.py -----------> DB 모델 정의부
│   ├── routes.py -----------> 엔드포인트와 핸들러
│   └── templates
│       ├── admin.html
│       ├── dashboard.html
│       ├── index.html
│       ├── manage_questions.html
│       ├── quiz.html
│       ├── quiz_list.html
│       └── results.html
├── entrypoint.sh ------------> ???
├── migrations ---------------> ???
│   ├── README
│   ├── alembic.ini
│   ├── env.py
│   ├── script.py.mako -------> ???
│   └── versions
│       ├── 4323820566e2_.py
│       ├── cf5540f43bf0_add_created_at.py
│       └── fd6b09e0e299_add_order_num_and_is_active_to_question.py
├── nginx.conf
├── pyproject.toml -----------> poetry 의존성
└── run.py
```

## Models

```mermaid
---
title: 심리테스트
---
erDiagram
	Participant {
		int id PK
		string name
		int age
		string gender
		date created_at
	}
	Admin {
		int id PK
		string username
		string password
	}
	Question {
		int id PK
		string content
		int order_num
		bool is_active
	}
	Quiz {
		int id PK
		int participant_id FK
		int question_id FK
		string chosen_answer
	}

	Participant ||--|{ Quiz : takes
	Question }o--|| Quiz : has
```

[[SQLAlchemy]]를 제대로 공부하지 않아서 cardinality 관계가 추론이 잘 안된다. `SQLAlchemy.relationship()`은 무엇을 정의하는 걸까?
