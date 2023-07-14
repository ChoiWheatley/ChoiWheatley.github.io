---
description:
aliases: 
tags: 
created: 2023-04-28T21:03:38
updated: 2023-07-11T15:21:09
title: chat page 작성하기 estsoft
---
- 결과물
- ![[Pasted image 20230428210356.png]]

# what i have learned

- css 파일을 두 개 이상 링크하는 것이 가능하다. 후자 우선 원칙에 따라 나중에 링크한 CSS파일이 우선순위가 높다.
- 연달아 오는 원소끼리 `+` 로 체이닝하여 별도의 상황에 대응할 수 있다. 위에 그림 보면 안녕이 세 번 연속된 말풍선이 상대적으로 더 가까운 것을 볼 수 있다.
```css
.user-chat + .user-chat {
  margin-top: 5px;
}
.ai-chat + .ai-chat {
  margin-top: 5px;
}
```
- [`overflow`](https://developer.mozilla.org/ko/docs/Web/CSS/overflow) 속성값을 활용하면 채팅길이가 일정 수준을 넘어설 때 스크롤 바가 생기게 만들 수 있다.