---
aliases: 
tags: 
description:
created: 2023-06-15T20:21:56
updated: 2023-07-15T21:33:03
title: python 필수 의존성만 추려내는 방법
---
- [bookstore/issues/29](https://github.com/ESTsoft-Book-Project/bookstore/issues/29)

[pipreqs](https://betterdatascience.com/python-pipreqs/#:~:text=Pipreqs%20works%20by%20scanning%20all.py%20files%20in%20a,a%20Python%20project%20folder%2C%20simply%20run%20this%20command%3A)는 프로젝트의 모든 `.py` 파일을 읽은 뒤 실제로 필요한 의존성만을 출력한다고 합니다.

설치법
---

```shell
pip install pipreqs
```

사용법
---

```shell
pipreqs - Generate pip requirements.txt file based on imports

Usage:
    pipreqs [options] [<path>]

Arguments:
    <path>                The path to the directory containing the application
                          files for which a requirements file should be
                          generated (defaults to the current working
                          directory).

Options:
    --use-local           Use ONLY local package info instead of querying PyPI.
    --pypi-server <url>   Use custom PyPi server.
    --proxy <url>         Use Proxy, parameter will be passed to requests
                          library. You can also just set the environments
                          parameter in your terminal:
                          $ export HTTP_PROXY="http://10.10.1.10:3128"
                          $ export HTTPS_PROXY="https://10.10.1.10:1080"
    --debug               Print debug information
    --ignore <dirs>...    Ignore extra directories, each separated by a comma
    --no-follow-links     Do not follow symbolic links in the project
    --encoding <charset>  Use encoding parameter for file open
    --savepath <file>     Save the list of requirements in the given file
    --print               Output the list of requirements in the standard
                          output
    --force               Overwrite existing requirements.txt
    --diff <file>         Compare modules in requirements.txt to project
                          imports
    --clean <file>        Clean up requirements.txt by removing modules
                          that are not imported in project
    --mode <scheme>       Enables dynamic versioning with <compat>,
                          <gt> or <non-pin> schemes.
                          <compat> | e.g. Flask~=1.1.2
                          <gt>     | e.g. Flask>=1.1.2
                          <no-pin> | e.g. Flask
```
