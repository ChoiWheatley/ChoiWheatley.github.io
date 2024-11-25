---
aliases: 
tags: 
description:
title: VSCode Django Unit Tests
created: 2024-11-26T00:39:11
updated: 2024-11-26T00:56:33
---
- ref: <https://code.visualstudio.com/docs/python/testing#_django-unit-tests>

---

## TL;DR

**글로벌 `settings.json`** 에 다음과 같은 필드를 추가하세요:

```json
{
	"python.experiments.optInto": ["pythonTestAdapter"]
}
```

**`.env`** 에 다음 키값쌍을 추가하세요:

```
MANAGE_PY_PATH='<path-to-manage.py>'
```

(Optional) **`.vscode/settings.json`** 에 원하는 장고 테스트 인자를 추가하세요:

```json
{
	"python.testing.unittestArgs": []
}
```

## 원인

- `./manage.py test`에서 통과하는 테스트 개수와 VSCode Testing에서 통과하는 테스트 개수가 다른 문제 발생.
- `./manage.py`를 사용하는 테스트는 `default` 데이터베이스를 만들어 `setUp`을 하고 테스트가 끝나면 삭제하지만 VSCode Testing은 그러지 않았던 것이다.
- VSCode는 `./manage.py` 명령을 실행하는 것이 아니었다. 그래서 로그를 확인해보았더니 `unittestadapter/discovery.py` 파일을 사용하고 있었다:

	```
	python /home/choiwheatley/.vscode-server/extensions/ms-python.python-2024.20.0-linux-x64/python_files/unittestadapter/discovery.py --udiscovery -p *test*.py
	```

- 이 파일은 `./manager.py` 파일의 존재를 모르기 때문에 단지 `unittest` 명령을 호출할 뿐이었고, `unittest` 명령은 장고 컨텍스트가 없기 때문에 임시 default 데이터베이스를 만들어야 한다는 정보를 모르는 것이다.

## 해결

ref에 따르면 `MANAGE_PY_PATH` 환경변수가 존재하면 장고 유닛테스트 컨텍스트임을 파악하고 일반 `unittest` 커맨드 대신에 `./manage.py test` 명령으로 대체한다. 이를 위하여 `.env` 파일에 `MANAGE_PY_PATH='<path-to-manage.py>'` 를 추가하면 알아서 읽어 환경변수에 추가하고 장고 유닛테스트 컨텍스트임을 파악하게 된다.

만약 `.env` 파일이 예상된 위치에 존재하지 않는 경우, `settings.py`에 정의할 수 있다.
