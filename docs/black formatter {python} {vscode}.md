---
description:
aliases: 
tags: 
created: 2023-06-08T10:28:56
updated: 2023-07-11T15:21:09
title: black formatter {python} {vscode}
---
포매터에는 여러 종류가 있습니다. autopep8, yapf, black이 있습니다. 개인적으로 black을 가장 선호합니다. 별다른 과정 없이 잘 돌아가거든요. [doc](https://marketplace.visualstudio.com/items?itemName=ms-python.black-formatter#usage)

## 설치방법

1. [black formatter](https://marketplace.visualstudio.com/items?itemName=ms-python.black-formatter)를 설치합니다. 
2. 커맨드 팔레트 `Ctrl+Shift+P`를 띄워 **Format document with...** 를 검색합니다. 
3. 드롭다운메뉴? 가 나올텐데, 거기에서 `black`을 선택하시면 됩니다.

## 옵션

### Format on save

You can enable format on save for python by having the following values in your settings: 저장을 누르면 자동으로 포매터가 실행되게 만들 수 있습니다. 근데 가끔 남이 쓴 코드를 지혼자서 막 바꿔대는 경우가 발생해서 평소엔 꺼놓다가 `Alt+Shift+S`를 눌러 내가 원할때에만 포매팅을 하게 만들어 놓기도 합니다.

아래 코드는 settings.json 파일을 열어 첫 번째로 열린 중괄호 사이에 추가하면 됩니다.

```json
  "[python]": {
    "editor.defaultFormatter": "ms-python.black-formatter",
    "editor.formatOnSave": true
  }
```