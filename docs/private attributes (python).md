---
description:
aliases: 
tags: 
created: 2023-05-18T00:12:05
updated: 2023-07-15T21:33:03
title: private attributes (python)
---

클래스 밖에서 **문법적으로** 접근이 불가능한 프라이빗한 변수를 만들 수 있다. 이름 앞에 `__`만 붙이면 끝임.

```python
class Car(object):
    __maxSpeed = 300
    maxPeople = 5
    def move(self, x):
        print(x, '의 스피드로 움직이고 있습니다.')
        print(self.__maxSpeed, '가 최고 속도입니다.')
    def stop(self):
        print('멈췄습니다.')

k5 = Car()
k5.move(10)
# k5.__maxSpeed #error
k5.maxPeople
```
