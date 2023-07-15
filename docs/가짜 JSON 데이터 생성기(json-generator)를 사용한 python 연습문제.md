---
description:
aliases: 
tags: 
created: 2023-05-18T00:03:18
updated: 2023-07-15T21:33:03
title: 가짜 JSON 데이터 생성기(json-generator)를 사용한 python 연습문제
---

[[json-generator]]의 도움을 받아 실제 JSON 형식으로부터 데이터를 추출하는 연습을 해 보는 것이 좋다고 한다. 어쨌든 생성한 회원들의 정보는 다음과 같을 때 
1. 회원들의 age 평균을 구하라.
2. 회원들의 남녀 성비를 구하라.

```python
data = [
  {
    "_id": "6459cadc09a2925f6ea0ca86",
    "age": 36,
    "eyeColor": "green",
    "name": "Jenna Case",
    "gender": "female"
  },
  {
    "_id": "6459cadc2bdc715ee28eae48",
    "age": 40,
    "eyeColor": "green",
    "name": "Vaughan Mcintyre",
    "gender": "male"
  },
  {
    "_id": "6459cadcb481717ac1215e7c",
    "age": 33,
    "eyeColor": "brown",
    "name": "Dudley Reed",
    "gender": "male"
  },
  {
    "_id": "6459cadc524800dc80d67c96",
    "age": 26,
    "eyeColor": "green",
    "name": "Samantha Gonzalez",
    "gender": "female"
  },
  {
    "_id": "6459cadc5ddd23c306cf09c1",
    "age": 20,
    "eyeColor": "blue",
    "name": "Mcmahon Deleon",
    "gender": "male"
  }
]
```

```python
### 1. 회원들의 age 평균을 구해주세요

# Using loop
my_sum = 0
for member in data:
    my_sum += member['age']
print(f'{my_sum / len(data):.2f}')

# Using map and sum
my_sum = sum(map(lambda x: x['age'], data))
print(f'{my_sum / len(data):.2f}')

# Using reduce
from functools import reduce
my_sum = reduce(lambda x, y: x + y['age'], data, 0)
print(f'{my_sum / len(data):.2f}')

### 2. 회원들의 남녀 성비

# using filter
len_male = len(list(filter(lambda x: x['gender'] == "male", data)))
len_female = len(data) - len_male
print("male: ", len_male / len(data) * 100, "%")
print("female: ", len_female / len(data) * 100, "%")

# using list comprehension
only_male = [x for x in data if x['gender'] == 'male']
len_male = len(only_male)
len_female = len(data) - len_male
print("male: ", len_male / len(data) * 100, "%")
print("female: ", len_female / len(data) * 100, "%")
```
