---
aliases: 
tags: 
description:
title: 2024-03-06 wishfunding API 설계, Enum Table 작성, RDS 인스턴스 생성
created: 2024-03-06T17:12:43
updated: 2024-03-10713T22:130:07:45
---
response status code를 두루뭉술하게 보내야 하는 이유: "id는 맞는데 비밀번호가 틀렸어요"  vs "id 또는 비밀번호가 틀렸어요"

## 2024-03-06 TODO

- [x] Jira에 [https://github.com/ChoiWheatley/wishlist_funding_dbdiagram](https://github.com/ChoiWheatley/wishlist_funding_dbdiagram) 연결하기
- [ ] 환불 플로우, 후원 취소 플로우 얘기해보기 [환불 플로우, 후원 취소 플로우](https://www.notion.so/73695140fa8e49f59aed00c2f965c0a0?pvs=21)
- [ ] 롤링페이퍼 API와 후원 API 병합
- [ ] `/api/gratitude/gratitudeId` OR `/api/gratitude/fundId` 둘 중에 하나로 결정하기 [감사인사 ID 의 이름 정하기](https://www.notion.so/ID-eb5d680f5eb34254bc51702523d5edb3?pvs=21)
- [ ] 댓글도 무한 스크롤을 지원하기 위한 pagination API 추가
- [ ] `user.email` , `user.profileImg`필드 추가
- [ ] 펀딩 상세 페이지에 Tab을 추가: 정보, 후기, 댓글
