---
aliases: 
tags: 
description:
title: natvis를 활용해 못생긴 vscode 디버거 예쁘고 똑똑하게 탈바꿈하기
created: 2024-01-11T17:06:53
updated: 2024-01-11T17:07:00
---
- [[C++]]
---
[Best debugging tool for C and C++](https://stackoverflow.com/questions/4072014/best-debugging-tool-for-c-and-c)

처음에는 빌드 옵션의 문제라고 생각했다. 그래서 디버그 심볼을 포함하여 컴파일 해주는 옵션인 `-g` 를 명시적으로 붙여봤지만 결과는 똑같았다. 그래서 vscode 의 디버그 옵션을 설정하는 `configure.json` 파일의 문서를 뒤져보았다.

[Configure launch.json for C/C++ debugging in Visual Studio Code](https://code.visualstudio.com/docs/cpp/launch-json-reference#_visualizerfile)

visualizer file 이라는 문단에 보면 natvis 라는 프레임워크를 사용할 수 있다고 나와있는데, natvis는 비주얼 스튜디오를 위한 디버그 시각화 view를 정의할 수 있다고 한다.

> 디버거는 사용자 지정 문자열 형식을 해석하는 방법을 인식하지 않으므로 텍스트 상자에 들어있는 문자열을 볼 수 없습니다.

사용자 지정 문자열 형식을 해석하는 방법을 Natvis가 제공하는 것이라고 볼 수 있다. 그렇다면 나도 natvis 파일을 하나 만들어 적어도 `std::string`, `std::vector`에 대해서 편한 시각화 디버깅을 수행할 수 있지 않을까?

[The Natvis framework provides custom views for native C++ objects](https://code.visualstudio.com/docs/cpp/natvis)

사실 기본 프리셋이 이미 존재했다! 이 사이트에 있는 Natvis 스키마를 복사해서 본인의 프로젝트에 `my.natvis` 파일에 붙여넣은 뒤, `configure.json` 파일에서 `"visualizerFile"` 속성과 `"showDisplayString"` 속성을 각각 추가하면 된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f37b9f2f-cc0b-483f-bd0c-d62734f3b46a/Untitled.png)
