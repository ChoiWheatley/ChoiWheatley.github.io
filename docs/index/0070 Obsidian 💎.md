---
description:
title: 0070 Obsidian 💎
created: 2023-02-10T18:11:21
categories: 
 - index
aliases: 
 - obsidian
 - 옵시디언
tags: [" index obsidian ", index, obsidian]
date created: Friday, February 10th 2023, 6:11:21 pm
date modified: Monday, February 27th 2023, 6:20:45 pm
updated: 2023-09-15T22:56:16
---
parent link: [[docs/index/index|index]]

---

# 유용한 정보 모음

- [config 싱크 기능의 부재](https://forum.obsidian.md/t/copy-settings-from-existing-vault-option/11082)
  - 세팅 싱크를 그냥 `.obsidian` 폴더로 하라는데? [link](https://forum.obsidian.md/t/copy-current-vault-settings-to-new-one/36134/2)
- [[각종 콜아웃들]]
- [[두 번째 뇌 만들기]]
- [[[Obsidian Basic Formatting Syntax]]
- [[obsidian으로 프리젠테이션 하기 - no boilerplate]]
- [[latex]]

# 플러그인

- [[dataview]]
- [[MarkDownload - Markdown Web Clipper - Share & showcase - Obsidian Forum|마크다운 웹 클리퍼 - 웹 브라우저 플러그인]]
- [[outliner and zoom {bullet list 기반 문서}]]
- [[Obsidian Linter]] | yaml frontmatter 자동생성, 표준 마크다운 형식으로 변환
- [Copy Url to Preview (iframe)](obsidian://show-plugin?id=convert-url-to-iframe) | 링크를 `<iframe>` 태그로 변환해준다.
- [[excalidraw, 가벼운 JSON 기반 다이어그램 편집기]]
- [[obsidian-git]]
- [[recent files obsidian plugin]]
- [[quick switcher obsidian plugin]]
- [[templater obsidian plugin, 맛탱이가 간 기본 템플릿 생성기 대체제]]

# 옵시디언 튜토리얼

- [[A Guide On Links vs. Tags In Obsidian - Knowledge management - Obsidian Forum|링크 vs 태그]]
- [[Use Obsidian Like a Pro]]
- [[A beginner's guide to using the Obsidian Notes application]]
- [[Quick Tip Footnotes in Obsidian - Obsidian Rocks]]

# 커뮤니티

- [[Has anyone created a bookmarking system in obsidian   ObsidianMD|북마크 관리자로써의 옵시디언]]
- [[tab reuse on link opening, tab management {Obsidian}]]

# 아무래도

옵시디언은 로컬 기반 문서 에디터의 한계를 벗어나기 전까진 말 그대로 어떤 구체적인 주제 하나에 대한 생각정리에만 사용이 될 것 같다. **미니멀** 하게 사용하는 편이 좋겠다. 거의 내가 생각하는 워크플로우를 위해서라면 항상 필수적으로 커뮤니티 플러그인을 설치해야만 하고 그것들이 꼭 완벽하게 돌아간다는 보장이 없기 때문에 메인으로 노션을, 휘발성 아이디어는 [[0005 Archieve 💾|구글킵]]을, 그리고 [[0005 Archieve 💾|구글킵]]에 메모한 글을 옵시디언으로 가져다 기억/아카이브하는 용도로 사용해야겠다.

- main: Notion
  - 포스팅 글(아무도 안 볼 거지만..)을 **노션**에 적는다.
  - 알고리즘 정리같은 건 확실히 노션이 좋다. 문서의 길이가 길어질수록 단순 마크다운 파일인 옵시디언에서의 메리트가 떨어지는듯.
- 휘발성 아이디어는 **[[0005 Archieve 💾|구글킵]]**을 애용한다.
  - 핸드폰에 최우선적으로 접근이 가능해서, 빠르기도 하고.
- 말 그대로 두 번째 뇌를 만드는 데 사용하는 **옵시디언**은 노트 간의 연결성에 더 집중을 가한다.
  - [[0005 Archieve 💾|구글킵]]에 메모를 쌓아두지만 말고 주기적으로 옵시디언으로 링크시켜둔다. 핸드폰 어플로도 적을 수는 있겠지만 읽기용으로만 사용하자.
  - 불렛리스트를 적극적으로 활용해보자.

# .obsidian 폴더 동기화 하기 (안전하게!)

1. 구식 볼트: remotely-save의 `Revoke Auth -> clean` 버튼을 차례로 눌러 현재 로컬에 캐시된 크레덴셜을 파기한다. 이때 MS의 remotely-save에 대한 권한 자체를 파기하는 건 내가 원하던 방식이 아니라 pass
2. 최신 볼트의 .obsidian 폴더를 그대로 복사해 구식 볼트에 붙여넣고 덮어쓴다.
3. 구식볼트를 그대로 앱을 실행시킨다.
4. PROFIT

****

# obsidian으로 블로그 만들기

[[Obsidian으로 블로그 만들기]]
