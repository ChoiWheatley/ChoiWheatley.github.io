---
description:
aliases: 
tags: 
created: 2023-05-29T21:43:28
updated: 2023-07-11T15:21:07
title: pytube 사용한 오픈소스 유튜브 다운로더
---
- 진짜 기본적인 코드는 다 만들어놨다. [youtube_downloader_converter](https://github.com/ChoiWheatley/youtube_downloader_converter)
- 근데 이걸 웹으로 호스팅 할 생각을 안했네? + API공개까지 해서 내가 굳이 pytube + ffmpeg 설치할 필요 없이 주어진 양식만 맞춰 GET 요청을 보내면 알아서 원하는 코덱과 원하는 컨테이너로 담아서 바로 다운로드 가능한 링크를 꽂아주는 그런 뭐시기 
- API를 노출하면 이걸 Apple 단축어나 다른 서비스에서 활용하기까지도 용이할 것 같다. 대표적으로 Firefox Addon으로 만들어 유튜브 프리미엄을 구독하지 않아도 영상을 보다가 바로 다운로드 받을 수 있게끔 만들어도 보고싶다.