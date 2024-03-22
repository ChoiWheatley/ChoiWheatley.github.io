---
description:
aliases: 
tags: 
created: 2023-04-24T20:53:42
updated: 2024-03-22T09:23:09
title: 0019 GitHub
---

# INDEX

<img src="http://ghchart.rshah.org/ChoiWheatley" alt="2016rshah's Github chart" />

- [[github code review]] ^j3p7rh
- [[git submodule 개념 잡기]] ^cevrke
- [[git rebase --interactive]]
- [[git commit message 규칙]]
- [[github blog 검색창 노출시키기 (TODO)]]

## RESOURCES

- [Notion / 알아서 잘 딱 깔끔하고 센스있게 정리하는 GitHub 핵심개념](https://paullabworkspace.notion.site/GitHub-435ec8074bcf4353afb947f601a030df)
	- [Notion / 알잘딱 / 2 Git 설치, 저장소 만들기, 명령어들](https://paullabworkspace.notion.site/2-Git-560d34629faf4d4cb19ad0462bbb4dc7)
	- [Notion / 알잘딱 / 3 Github 세팅](https://paullabworkspace.notion.site/3-GitHub-5af717e53119443d9de827abaa710ced)
		- `git pull` 시 누군가 이미 코드를 수정했을 경우(current branch is *behind* the remote)에 대처방법 [git pull --rebase - wiki](https://www.git-scm.com/docs/git-pull) 
			- [`git pull --no-rebase`](https://www.git-scm.com/docs/git-pull#Documentation/git-pull.txt---rebasefalsetruemergesinteractive) => 로컬 main과 원격 main을 서로 다른 브랜치로 보고 병합한다.
			- [`git pull --rebase`](https://www.git-scm.com/docs/git-pull#Documentation/git-pull.txt---rebasefalsetruemergesinteractive) => 시간 순서대로 병합한다. rebase의 뜻은 *새로운 평가 기준을 설정하다*. 라는 말임. 
			- `git pull`은 사실 여러개의 명령을 하나로 뭉친것이었다... [`git fetch`](https://www.git-scm.com/docs/git-fetch) -> `git rebase` OR `git merge` 구분 기준은 병치하는 브랜치 사이에 관계를 보고 결정한다고 한다. 그 기준까지 알아야 하나? 
	- [4.branch](https://paullabworkspace.notion.site/4-Branch-7552c2a8c2b340ce9622a8a6364dc47a)
		- 브랜치를 사용하는 4가지 이유
			1. 목적과 속도가 다른 작업들을 독립적으로 다루기 위하여
			2. 잦은 병합 및 충돌은 머리가 아프기 때문
			3. 브랜치가 제공하는 PR기능을 활용하기 위하여
		- `HEAD`에 대하여 설명하세요
			- working tree의 가장 마지막 커밋을 가리키는 포인터. 
				- [?] HEAD는 unstaged space에 있나요, staged space에 있나요?
		- working tree가 무엇인가요?
			- `git init`이나 `git clone`등을 사용하여 파일시스템에 저장된 로컬 디렉토리를 의미합니다. 
- [learn git branching with visual tutorial](https://learngitbranching.js.org/) 

## git 관련 예상 면접 질문

- [[git과 github의 차이점에 대하여 말하세요]]
- [[git이 작동하는 원리를 간략하게 소개하세요]]
- [[버전 분산 관리가 필요한 이유는 무엇인가요]]

---

## github pages 생성 

- 정적 웹 사이트 호스팅 서비스인 pages를 생성할 수 있다.![[스크린샷 2023-04-24 21.03.57.png]]
- 아무 리파지토리 생성하여 settings > pages > branch 설정하고 새로고침 몇 번만 하면 *Your site is live at (<https://choiwheatley.github.io/my-first-pages/>)* 라고 뜨는데, 이때 해당 링크 따라 들어가면 된다.
