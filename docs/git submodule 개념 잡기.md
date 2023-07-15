---
description:
aliases: 
tags: 
created: 2023-04-25T15:41:00
updated: 2023-07-15T21:33:04
title: git submodule 개념 잡기
---
- 지금까지 알게된 것들 일단 싸지르기
	- 일단 모든 리포지토리는 적어도 한 개 이상의 파일과 커밋이 존재해야 어떤 작업이든 수행할 수 있다.
	- parent repository 안에서 다른 리포지토리를 서브모듈로 추가해야 할 상황에서

```shell
git submodule add <repository-link>
```

- 
- [ormi-master](https://github.com/ChoiWheatley/ormi-2023-04-26/tree/3ea3198848a705061d0436b1a6b64ef05a9022d6) 에 서브모듈 여러개를 만들어 링크시켜놓았다. 신기한게, 서브모듈을 커밋하고 푸시했을지라도, 자동으로 부모 리포가 서브모듈의 `main` 브랜치를 알아서 가리키지 않는 것으로 보인다. 따라서 그때그때 부모리포에서도 커밋 및 푸시를 진행해야 했다. 
	- 부모리포가 무조건 서브모듈의 최전방 커밋을 가리키도록 만드는 방법은 없을까?
	- [스택오버플로](https://stackoverflow.com/questions/1030169/pull-latest-changes-for-all-git-submodules#1032653)
	- [블로그글 - 깃 액션](https://tommoa.me/blog/github-auto-update-submodules/)
