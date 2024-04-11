---
description:
aliases: 
tags: 
created: 2023-04-25T09:26:49
updated: 2023-07-15T21:30:22
title: 20230425 estsoft - Network, VSCode settings, emmet syntax, meta
---

# 쌩기초

- [수업을 시작하기 전 알아야 할 기초 상식](https://paullabworkspace.notion.site/8444faa97f724967b6ee1da374fd0d07)
- [숭실 2021-2학기 Network 수업 원드라이브](https://1drv.ms/f/s!AgE-lhMulmhxg5RBWrSyNkKMQvgsnw?e=8fpSKZ) ^x5b0l8

노트북에서 `www.naver.com` 을 검색했을 때 어떤 일이 일어날까? 
1. 나는 이거의 주소를 몰라 => 우리집 공유기와 아파트 라우터를 타고 DNS 서버에 당도한다. 
2. DNS 서버에 해당 문자열을 질의한다. 매핑이 된 ip주소를 서버는 노트북으로 발송해준다.
3. 노트북은 해당 매핑을 캐싱하고 다시 진짜 IP주소를 향해 신호를 보낸다.

- NAT: 외부에서 요청한 패킷을 한꺼풀 벗겨내 내부IP로 연결해주는 녀석.
- DDX: (Distributed) Denial of Service 공격 방지해주는 녀석. 임계치 기반 방어
- IPS: 행위 기반 방어
- Firewall: IP-Port 기반 방어

- App Server == Django
	- 로그인과 같은 행위에 대하여 DB작업을 해야 하는 경우
- Web Server == Apache, nginx
	- static한 컨텐츠 
- DB == MongoDB, MySQL, ...

---

# VSCode 환경설정 및 단축키

- https://paullabworkspace.notion.site/VSC-e89ec679f7a44c2882012ec49f164ced

- 같은 패턴이 반복되는 것은 **스니펫**으로 만들어 광명찾자.
	- 뿐만 아니라 회사에서 컨벤션을 정의하여 스니펫으로 공유하기도 한다.
- 유용한 확장
	- htmltagwrap : Alt + w를 누르면 기본 p 태그로 감싸줌 
		- 내가 정의한 단축키와 중복이 된다. 
	- gitmoji: git 커밋에 이모지를 사용할 수 있게 해준다.
	- indent-rainbow: 들여쓰기마다 색을 다르게
	- (Python) pylance, formatter autopep8, flake8
	- (설치) auto rename tag (추천) : 태그 닫기 자동 수정
	- (설치) rainbow csv (추천) : CSV 잘 보이도록

# emmet 문법

https://docs.emmet.io/cheat-sheet/
- lorem 이거 신기하네
- [한글입숨](http://hangul.thefron.me/)도 있다. 무의미한 한글로 채워넣는다.

# Github

[[0019 Git ᛘ]]

# 제안

- 고급반 운영할 계획, (ex. 카카오 공채) 질문 리스트 공유하여 쌓아놓고 서로 모의면접도 볼 수 있게끔.
- 중급자를 위한 자료는 미리 드리려고 한다. 인프런 중급자 파이썬 
- 모자란거 2%를 채우려고 하지 말고 차라리 다른 98%에 시간을 투자할 것.
