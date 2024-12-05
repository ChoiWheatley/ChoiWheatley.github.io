---
aliases: 
tags: 
description:
title: jest를 사용할땐 import를 relative path로 작성하거나 moduleNameMapper를 설정해야한다.
created: 2024-12-05T18:05:40
updated: 2024-12-05T18:23:38
---
- ref:
	- [[0018.3 jest testing framework for Node.js]]
	- Resolve file paths in Jest: <https://stackoverflow.com/questions/49616333/resolve-file-paths-in-jest#56298746>

---

절대경로라는 말이 UNIX의 절대경로라는 말과는 조금 다르게 통용되는 것 같다. `<rootDir>` 부터 시작하는 import 경로를 절대경로라고 칭하고, `./` 혹은 `../`으로 시작하는 경로를 상대경로라고 칭하는 것 유념하자.

## 문제상황

Jest 코드를 작성하고 모든 디펜던시 문제를 해결하고 기분좋게 `toBeDefined` 테스트를 실행한 순간 다음 에러 메시지가 나를 반겼다:

```
 Cannot find module 'src/enums/error-code.enum' from 'src/filters/giftogether-exception.ts'
 
      1 | import { HttpException, HttpStatus, Injectable } from '@nestjs/common';
    > 2 | import { ErrorCode } from 'src/enums/error-code.enum';
        | ^
      3 | import { ErrorMsg } from 'src/enums/error-message.enum';
      4 |
      5 | export class GiftogetherException extends HttpException {
```

원래라면 문제없이 임포트에 성공해야 했을 `ErrorCode`, Jest는 제대로 읽어내지 못하는 것 같아보였다. 그래서 스택오버플로 질의응답을 확인했더니 `moduleNameMapper`를 이용하면 된다고 한다. 아래는 예제 매핑이다:

```json
// package.json
{
  "jest": {
    "modulePaths": ["/shared/vendor/modules"],
    "moduleFileExtensions": ["js", "jsx"],
    "moduleDirectories": ["node_modules", "bower_components", "shared"],

    "moduleNameMapper": {
      "^react(.*)$": "<rootDir>/vendor/react-master$1",
      "^config$": "<rootDir>/configs/app-config.js",

      "\\.(css|less)$": "<rootDir>/__mocks__/styleMock.js",
      "\\.(gif|ttf|eot|svg)$": "<rootDir>/__mocks__/fileMock.js"
    }
  }
}
```

나는 webpack을 사용하지 않으니 그저 `"src/"`로 시작하는 매핑만 잘 정의하면 될 것으로 보인다.

```json
"moduleNameMapper": {
  "^src/(.*)$": "<rootDir>/src/$1"
},
```

## 해결

![[Screenshot 2024-12-05 at 18.23.31.png]]
