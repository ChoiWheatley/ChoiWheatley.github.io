---
aliases: 
tags: 
description:
title: dollar sign $ in {haskell}
created: 2024-10-24T12:08:32
updated: 2024-10-24T12:15:31
---
ref: <https://learnyouahaskell.com/higher-order-functions#function-application>

```haskell
f $ x = f x
f $ x $ y = f (x y)
```

`$` 연산자는 function application 이라고 부르는데, 하스켈에서 가장 낮은 연산자 우선순위를 갖는다. 따라서 기본적인 연산자 결합순서가 뒤집혀 오른쪽부터 결합하게 된다.

```haskell
f x y = (f x) y
```

function application 없이 쓴 구문보다 더 깔끔해지는 경향이 있다. 예를 들면 아래 두 문장은 같다.

```haskell
sum (filter (> 10) (map (*2) [2..10]))
sum $ filter (> 10) $ map (*2) [2..10]
```
