---
aliases: 
tags: 
description:
title: Raycast, Alternator of Spotlight and Alfred
created: 2024-11-18T00:10:29
updated: 2024-11-18T00:21:51
---
- ref: <https://www.raycast.com>
- TLDR: Raycast는 macOS용 강력한 애플리케이션 런처로, 다양한 내장 기능과 확장성을 통해 생산성을 향상시킵니다.
---

## GPT Summarize

Raycast는 macOS용 강력한 애플리케이션 런처로, 기본 Spotlight 기능을 대체하며 다양한 생산성 도구를 제공합니다. 2020년에 출시된 Raycast는 빠른 애플리케이션 실행, 파일 검색, 클립보드 관리, 플로팅 노트, 계산기 등 다양한 내장 기능을 통해 사용자 경험을 향상시킵니다.

또한, Raycast는 확장 가능한 플랫폼으로, 사용자는 스토어에서 다양한 오픈 소스 확장을 설치하여 기능을 확장할 수 있습니다. 예를 들어, GitHub 리포지토리 관리, Spotify 제어, Google 번역을 통한 언어 번역, 브라우저 북마크 접근, Jira와의 연동 등이 가능합니다.

Raycast는 사용자 인터페이스의 일관성과 키보드 중심의 조작 방식을 통해 효율적인 워크플로우를 지원합니다. 또한, AI 통합 기능을 통해 ChatGPT와 같은 인공지능 도구를 빠르게 활용할 수 있으며, 이는 유료 기능으로 제공됩니다.

최근 Raycast는 Windows와 iOS 버전 출시를 발표하며 플랫폼 확장을 계획하고 있습니다. 이는 더 많은 사용자가 Raycast의 생산성 도구를 다양한 환경에서 활용할 수 있도록 지원하기 위함입니다.

## My Config

알프레드처럼 검색 쿼리를 추가할 수 있다.

![[Screenshot 2024-11-18 at 00.12.47.png]]

Extensions에 Quicklinks에 들어가면 원하는 쿼리를 추가할 수 있다. `{Argument}` 를 Links에 추가하면 탭 키를 눌러 검색으로 넘어갈 수 있다. 예를 들면 유튜브 Link는 아래와 같이 작성해놓았다:

```
https://youtube.com/results?search_query={argument name="Query"}
```

![[Screenshot 2024-11-18 at 00.14.52.png]]

![[Screenshot 2024-11-18 at 00.17.07.png]]

## 게임 하면서 모르는 영단어 찾기

게임은 주로 전체화면인 경우가 많다. 그래도 Raycast는 정상적으로 열리고 그 안에서 사전 검색, 구글 번역기(익스텐션)를 지원한다!! 아래 내가 실제로 게임을 플레이하면서 Raycast로 영단어를 검색하고 정리한 모습이다:

![[Screenshot 2024-11-17 at 17.03.56.png]]
