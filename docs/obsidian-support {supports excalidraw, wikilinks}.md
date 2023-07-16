---
aliases: 
tags: 
description:
title: obsidian-support {supports excalidraw, wikilinks}
created: 2023-07-16T00:59:21
updated: 2023-07-16T17:42:17
source: https://github.com/ndy2/mkdocs-obsidian-support-plugin
---

## excerpt

[mkdocs-obsidian-support-plugin](https://github.com/ndy2/mkdocs-obsidian-support-plugin)은  아직 경로를 작성하여야 하는 문제가 있다. roamlinks 플러그인의 [소스코드](https://github.com/Jackiexiao/mkdocs-roamlinks-plugin/blob/master/mkdocs_roamlinks_plugin/plugin.py)를 참조하여 경로와 독립적인 파일참조 기능을 제안하자.

## `convert_excalidraw` function

하이라이트된 코드를 보면 인자의 `excalidraw_path`를 가지고 파일을 찾고 있음을 알 수 있다. 하지만 이것은 옵시디언에서 사용한 media wiki의 유스케이스와는 거리가 멀다. `[[mkdocs]]` 이렇게 검색하지, 누가 `[[docs/mkdocs]]` 이렇게 레퍼런스 함? 비단 `ExcalidrawConvert`만의 문제가 아니라 `AbstractConversion`의 공통적인 문제점이다. 나는 이 `AbstractConversion` 추상클래스의 기능을 확장하여 경로도 포용하고, 단순파일이름도 포용하는 코드를 제안하고자 한다.

<iframe src="https://github.com/ndy2/mkdocs-obsidian-support-plugin/blob/e52be7a01eceac657209b207db9f17d0e45c3049/obsidian_support/conversion/excalidraw.py#L24-L28" allow="fullscreen" allowfullscreen="" style="height:100%;width:100%; aspect-ratio: 16 / 9; "></iframe>

## roamlinks plugin

아래를 참고하면 그냥 `os.walk`를 사용하여 전수조사를 하고있다. 쫌... 비효율적이긴 한데 어차피 빌드 한 번만 수행하면 되니까 상관없겠지?

<iframe src="https://github.com/Jackiexiao/mkdocs-roamlinks-plugin/blob/8a8aaa95e4a33b3f1be6334c9a8f4bacfc438fc5/mkdocs_roamlinks_plugin/plugin.py#L66" allow="fullscreen" allowfullscreen="" style="height:100%;width:100%; aspect-ratio: 16 / 9; "></iframe>
