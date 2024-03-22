---
aliases: 
tags: 
description:
title: 2024-03-22 Git, Github 극초반 세팅 특강
created: 2024-03-22T14:35:56
updated: 2024-03-22T15:35:32
---

## RESOURCE

- [위니브 / 알잘딱 깃 / 2. Git](https://paullabworkspace.notion.site/2-Git-560d34629faf4d4cb19ad0462bbb4dc7)
- [위니브 / 알잘딱 깃 / 3. GitHub](https://paullabworkspace.notion.site/3-GitHub-5af717e53119443d9de827abaa710ced)

## 목표

어차피 Git, Github 강의가 존재하기 때문에 자세한 원리는 그때가서 배웁시다. 이번 특강은 과제 제출을 위하여 필수적으로 알아야 하는 것들만 심플하게 짚고 넘어갑니다.

키워드: git, gh, repository, vscode

## 특강 안내 순서

- 설치
	- brew 설치 (linux, mac) [[homebrew]]
	- git 설치 `brew install git`
	- gh 설치 `brew install gh`
	- 버전 확인 및 초기 설정
- git 필수 커맨드
	- 일단 리포지토리 연결않고 `git init`을 사용하여 로컬에서 add, commit까지만 수행
	- `git status` `git diff` `git log` 사용해보기
- github
	- 새 리포지토리 만들기
	- `git remote add` 명령어를 사용하여 원격 리포지토리 연결하기
	- `git pull origin main` 해보기
	- `git push -u origin main` 해보기
- github2
	- 클론 해오기
	- 웹에서 수정한 뒤 pull 해보기
	- 파일 수정 후 add, commit, push까지
	- [!] 아마도 credential 때문에 문제가 발생할 것임
- credentials
	- ssh key 등록
	- `gh auth login` 로도 진행
- vscode
	- `.git` 이 들어있는 디렉토리 열기
	- `.git` 이 들어있지 않는 디렉토리 열기 → GUI `Initialize Repository` & `Publish to GitHub`
