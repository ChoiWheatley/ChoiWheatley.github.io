---
aliases: 
tags: 
description:
title: SQLAlchemy
created: 2024-05-05T20:54:54
updated: 2024-05-05T21:57:24
---

## Before reading documents

- `db = SQLAlchemy()`: 흔한 패턴인듯. db 객체를 만들어서 
---
- `db.Model`: 얘를 상속하는 클래스는 db 모델이 되는 것 같다.
- `db.Column()`: 컬럼을 정의할때 쓰는군. 인자로 `db.String, db.Boolean, db.Integer` 같은 타입을 정의할 수 있고, `primary_key=True` 키워드 인자로 컬럼 속성을 정의할 수 있다.
	- keyword arguments
		- `primary_key=bool`
		- `default`
- `db.ForeignKey()`: FK 연관관계를 정의할 때 사용한다. 인자로 문자열이 들어가는데, relationship의 이름과 해당 테이블에서의 PK에 해당하는 컬럼을 적는 것 같다.
	- 예: `participant_id = db.Column(db.Integer, db.ForeignKey("participant.id"))`
- `db.relationship()`: 위에서 연관관계를 정의할 때 애도 필요하다. `db.ForeignKey`의 인자로 들어가는 문자열의 . 왼쪽에 나타나는 프로퍼티의 이름.
	- args
		1. 연관 테이블 이름 (str)
		2. `backref` keyword argument (str)

## At the first glance

<https://docs.sqlalchemy.org/en/20/intro.html>

SQLAlchemy의 패러다임: SQL과 ORM

> Core와 SQL 표현 언어를 사용하는 것은 데이터베이스에 대한 스키마 중심적인 관점을 제공하며, 변경 불가능성을 중심으로 한 프로그래밍 패러다임을 갖추고 있습니다. 반면에 ORM은 이를 기반으로 데이터베이스에 대한 도메인 중심적인 관점을 제공하며, 명시적으로 객체 지향적이고 변경 가능성에 의존하는 프로그래밍 패러다임을 가지고 있습니다. 관계형 데이터베이스 자체가 변경 가능한 서비스이기 때문에 Core/SQL Expression 언어는 명령 중심적인 반면, ORM은 상태 중심적입니다. 간단히 말해, Core/SQL Expression은 명령을 중심으로 데이터베이스를 다루는 반면, ORM은 상태를 중심으로 데이터베이스를 다룬다는 것입니다.  

engine은 session에 의해 관리가 된다. Engine은 데이터베이스 연결을 관리하고 저수준의 작업을 처리하는 반면, Session은 ORM과 객체와의 상호작용 및 트랜잭션 관리를 담당합니다.

![[sqla_arch_small.png]]
