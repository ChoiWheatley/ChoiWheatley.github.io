---
description:
aliases: 
tags: 
created: 2023-05-24T09:26:36
updated: 2024-021-4T208T7:4915:18:23
title: Obsidian으로 블로그 만들기
---
- [[0070 Obsidian 💎|obsidian]]
---

## Concepts

- 깃허브 마크다운 포맷 + double-bracket 레퍼런스 지원
- dataview 쿼리 결과를 링크로 변환
- back-link & outgoing-link 결과를 볼 수 있게

## Search

- ["Obsidian+Github Pages" for digital gardeners? {forum}](https://forum.obsidian.md/t/obsidian-github-pages-for-digital-gardeners/2622)
  - `[[`, `]]` 포맷은 media wiki라고 한다. 일반 `a` 태그와는 조금 다르기 때문에 일반 리포지토리 안에 마크다운 파일에서는 동작하지 않는대. 하지만 GitHub wiki를 사용하면 Media wiki가 동작한다고 한다. 따라서 옵션이 두 가지가 있다.
  - 모든 옵시디언 링크를 media wiki에서 마크업 `a` 태크로 만들 것
  - github wiki를 그냥 내 블로그로 만들 것

- [How to generate a website from GitHub wiki pages {SOF}](https://stackoverflow.com/questions/16753586/how-to-generate-a-website-from-github-wiki-pages)
  - Hugo 따위를 사용하여 마크다운 파일들을 웹사이트로 만들어주는 일을 사용하라.
  - md files -> Hugo -> html files -> github pages -> netlify -> DNS
  - [hugo with double brackets link {GH Issue}](https://github.com/gohugoio/hugo/issues/3606)
  - [obsidian-to-hugo {GH}](https://github.com/devidw/obsidian-to-hugo) 를 사용하면 옵시디언의 `[[]]` 양식을 자동으로 Hugo 링크로 변경해준다고 한다. 이것을 깃허브 액션에 추가한다면 내가 생각하는 자동 빌드가 될 것 같다.

- Obsidian -> github -> wikidocs 정적 사이트 형태로 컴파일  -> 깃허브 이슈 형태로 댓글창  -> netlify -> DNS -> 배포
  - 잠깐만... 이걸 한 번에 진행시켜주는 서비스를 만들 수도 있을 것 같은데? [[Service that deploys user's blog from markdown and media wiki]]

___

# Scraps

- [15 Best wiki sw tools for 2023](https://document360.com/blog/wiki-software/)
- <https://docs.gitbook.com/> GitBook
  - Free, Open Source Service
- <https://js.wiki/>
  - Free, Open Source
- [mdbook](https://rust-lang.github.io/mdBook/)
- 마크다운 기반 위키생성 프로그램
- [Obsidian webpage export](https://github.com/KosmosisDire/obsidian-webpage-export)
	- 얘도 그래프 뷰, 데이터뷰, `!` 미리보기 등을 지원하는 프로젝트이다. mkdocs와 호환은 안될듯.
- [gatsbyjs](https://www.gatsbyjs.com/docs/tutorial/getting-started/part-1/)
	- 마크다운 변환 + 별도의 클라우드 마련 및 배포 자동화 사전제공
	- [gatsby-remark-obsidian](https://www.gatsbyjs.com/plugins/gatsby-remark-obsidian/) 플러그인으로 옵시디언 레퍼런스 형식을 지원하기는 하나 옵시디언의 플러그인 형식은 지원 안한다.

# Jekyll Integration

- [Notenote.link - jekyll theme for obsidian {forum.obsidian}](https://forum.obsidian.md/t/notenote-link-publish-your-obsidian-notes-with-jekyll-for-free/7951)
- [Setting up your own digital garden with Jekyll {Maxime Vaillancourt}](https://maximevaillancourt.com/blog/setting-up-your-own-digital-garden-with-jekyll)
- [[jekyll escaping liquid tags]]

Hugo는 `[[]]` 문법을 지원하지 않는다. 나는 굳이 변환툴을 사용하려고 하지 않는다. 따라서 해당 문법을 지원하는 jekyll을 사용하는 것이 정답인 것 같다. (나중에 내가 하나를 새로 만들던가)

Jekyll에도 굉장히 많은 테마가 있고, 어떤 테마는 심지어 백링크 기능도 지원한다. 이 기능을 활용하면 옵시디언과 유사한 룩앤필을 연출할 수 있을 것으로 기대된다.

아, canvas 적용이 안된다. 일단 당장은 canvas를 무시하게 만들어주자.
___

# MkDocs

[[mkdocs]] 으로 가세요

___

# Digital Garden Plugin

<https://dg-docs.ole.dev/>

허허.. 내가 mkdocs로 뺑이치고 있었는데 그냥 플러그인 자체로 퍼블리싱을 지원하는 녀석이 있었넹... 😅

graph view, back link, search with Ctrl+K, nav bar 등등... 있을 건 다 있다...!

## Google 검색창 노출시키기

[[github blog 검색창 노출시키기]]
