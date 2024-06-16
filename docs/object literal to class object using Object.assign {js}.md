---
aliases: 
tags: 
description:
title: object literal to class object using Object.assign {js}
created: 2024-06-16T17:45:48
updated: 2024-06-16T17:46:15
---
예제:

```typescript
const {userImg, userAcc, defaultImgId, ...userInfo} = userDto;
const user = new User();

// TODO 중복값에 대한 예외 처리 (userPhone, userNick)
Object.assign(user, userInfo);
```
