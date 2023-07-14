---
description:
aliases: 
tags: 
created: 2023-05-18T00:19:29
updated: 2023-07-11T15:21:08
title: file-io (python)
---
# Write File

```python
f = open("python.txt", "w")

s = "hello\nworld"
f.write(s)

f.close()
```

연습문제: 두 리스트를 활용하여 JSON 파일을 하나 만들어보라.

```python
data1 = ["one", "two", "three"]
data2 = [10, 20, 30]

# https://stackoverflow.com/a/3229493


def pretty(d):
    cnt = len(d)
    ret = "{\n"
    for key, value in d.items():
        ret += '\t"' + str(key) + '": '
        ret += str(value) + ("," if cnt > 1 else "") + "\n"
        cnt -= 1
    ret += "}"
    return ret


res = dict(zip(data1, data2))

print(pretty(res))

f = open("data.json", "w")
f.write(pretty(res))
f.close()

```

```python
# 사실 json.dump 쓰면 되지롱~~
import json

f = open("data2.json", "w")
json.dump(dict(zip(data1, data2)), f, indent="\t")
f.close()
```

# Read File

```python
for line in open("python.txt", "r").readlines():
    print(line, end="")
f.close()
```

# 파일 열기와 닫기를 동시에 `with`

```python
with open('test.txt', 'w') as f:
	f.write('Life is too short, you need python')
```