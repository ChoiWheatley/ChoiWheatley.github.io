---
aliases: 
tags: 
description:
title: python은 분기예측을 하는가 {sof}
created: 2023-08-14T16:43:16
updated: 2023-08-14T16:45:09
---
- [sof link](https://stackoverflow.com/questions/53005367/has-python-got-branch-prediction)

# Tl;DR

인터프리터마다 다르다. 그리고 분기예측은 HW 단에서 수행하는 것이지, 추상 레이어 위에서 돌아가는 파이썬은 분기예측이라고 표현하기는 뭐하다. 다만, JIT 인터프리터의 종류에 따라서 분기를 사전에 예측하여 최적화를 수행하는 경우(PyPy, Jython 등)는 있다
