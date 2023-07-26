---
aliases: 
tags: 
description:
title: mkdocs
created: 2023-07-14T10:20:59
updated: 2023-07-15T21:33:04
---

# Themes and Plugins

- [mkdocs material](https://github.com/squidfunk/mkdocs-material) 테마
- [mkdocs/catalog](https://github.com/mkdocs/catalog) mkdocs 플러그인, 테마 모음집
  - [roamlinks plugin](https://github.com/Jackiexiao/mkdocs-roamlinks-plugin) 는 wikidocs 문법을 지원하고 헤딩도 지원한다. 심지어 이미지 크기조절도 가능.
- [backlinks](https://pypi.org/project/mkdocs-backlinks/) 역시, 백링크도 지원하는구만
- [dev.to](https://dev.to/ar2pi/publish-your-markdown-docs-on-github-pages-6pe) 친절한 튜토리얼, `mkdocs`, `mkdocs gh-deploy` 명령도 알려준다.
  - GitHub Page를 위한 리포 안에 실제 문서들이 들어있는 서브모듈을 담아놨다.
- [[obsidian-support {supports excalidraw, wikilinks}]]
- [mkdocs-awesome-pages-plugin](https://github.com/lukasgeiter/mkdocs-awesome-pages-plugin#combine-custom-navigation--file-structure) 을 사용하면 `nav`를 서브폴더에 있는 내용을 자동으로 추가할 수 있다.

repo 이름을 `GITHUB_USERNAME.github.io`로 지으면 그 자체가 하나의 깃허브 페이지가 되는구나. 이 사실을 잘 파악하면 md전용 리포에 푸시 이벤트가 발생했을 때 배포전용 리포에 `gh-deploy`하도록 자동화 할 수 있을 것 같다.
