---
description:
aliases: 
tags: 
created: 2023-06-08T09:45:15
updated: 2023-07-11T15:21:07
title: pylint-django {pip} {vscode}
---
[[0014 Python 🐍#Fast Index]]에 적어놓자.

이 문서는 pip를 사용하여 pylint, pylint-django를 설치하고 vscode 내에서 이를 연결하고 다른사람과 동일한 린트옵션을 가져갈 수 있는 방법에 대해 설명합니다. | [Linting Python in Visual Studio Code {doc}](https://code.visualstudio.com/docs/python/linting) | [pylint-django {pip}](https://pypi.org/project/pylint-django/) | [pylint {pip}](https://pypi.org/project/pylint/) |

## Enable linting

vscode 커맨드 팔레트 (`Ctrl+Shift+P`)를 열어 **Python: Select Linter**를 치면 현재 파이썬 환경 (로컬 환경 or venv 환경)에 알아서 `pylint`를 설치해 준다고 합니다. 완전개꿀. 

만약 설치 프롬프트가 뜨지 않거나 자동으로 설치하도록 만들어주지 않는다면 `pip install pylint`를 직접 커맨드에 쳐서 설치하면 됩니다.

## Linting settings

필수적인것으로 보이는 놈들만 적습니다. `Ctrl+,`를 눌러 나오는 환경설정의 검색창에 대충 `python lint` 까지만 적으면 나오는 목록에서 추렸습니다.
- `Python > Linting: Enabled` => 기본으로 꺼져있는데 켜주세요. 
- `Python > Linting: Lint On Save` => 기본으로 켜져있기는 한데 확인해주세요.
- `Python > Linting: Pylint Args` => pylint 관련 옵션을 더 추가할 수 있습니다. 좀 있다 이 부분을 건들일 겁니다.

## Install `pylint-django`

`pip install pylint-django`를 실행하여 설치하시면 됩니다. 다시 vscode로 돌아와 `Pylint Args`부분에 다음 내용을 추가해주세요.

```
--load-plugins=pylint_django
```

![[Pasted image 20230608102050.png]]
