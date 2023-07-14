---
description:
aliases: 
tags: 
created: 2023-06-05T22:22:05
updated: 2023-07-11T15:21:09
title: Changing to a custom user model mid-project
---
- [doc](https://docs.djangoproject.com/en/4.2/topics/auth/customizing/#substituting-a-custom-user-model)

프로젝트 중간에 `AUTH_USER_MODEL` 을 변경하는 행위는 테이블 스키마를 변경하는 행위가 되므로 많은 수작업을 필요로 한다. 

> Due to limitations of Django’s dynamic dependency feature for swappable models, the model referenced by AUTH_USER_MODEL must be created in the first migration of its app (usually called 0001_initial); otherwise, you’ll have dependency issues.
> 장고의 교체 가능한 모델에 대한 동적 의존시스템의 한계로 인해 `AUTH_USER_MODEL`에서 참조하는 모델은 `0001_initial`에서 생성된다. 만약 이게 서로 충돌한다면 종속성 문제가 발생한다.

바로 내가 경험했던 무시무시한 `CircularDependencyError`가 발생하는 것!

> If you see this error, you should break the loop by moving the models depended on by your user model into a second migration. (You can try making two normal models that have a `ForeignKey` to each other and seeing how `makemigrations` resolves that circular dependency if you want to see how it’s usually done.)