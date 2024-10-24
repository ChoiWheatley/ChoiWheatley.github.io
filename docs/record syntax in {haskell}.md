---
aliases: 
tags: 
description: 
title: record syntax in {haskell}
created: 2024-10-24T12:22:16
updated: 2024-10-24T12:35:26
---
ref: <https://learnyouahaskell.com/making-our-own-types-and-typeclasses/#record-syntax>

하스켈의 record는 C의 struct와 같은 의미이다. 기본 타입을 가지고 사용자 정의 타입을 만들 수 있는데, `data` 타입을 정의할때 튜플(❓)로 정의할 경우 속성의 순서와 의미를 별도로 알고 있어야 한다는 단점이 있다. 따라서  각 속성에 이름을 정의하는 문법인 record를 사용하면 편하다.

```haskell
data Person = Person { firstName :: String
                   , lastName :: String
                   , age :: Int
                   , height :: Float
                   , phoneNumber :: String
                   , flavor :: String
                   } deriving (Show)
```

C의 struct와는 다르게, record에 정의된 각 속성은 함수다. 단지 record 타입을 인자로 받아 `::` 오른쪽에 있는 타입의 값을 리턴하는 함수일 뿐이다.

```haskell
ghci> :t firstName
firstName :: Person -> String
ghci> :t lastName
lastName :: Person -> String
```
