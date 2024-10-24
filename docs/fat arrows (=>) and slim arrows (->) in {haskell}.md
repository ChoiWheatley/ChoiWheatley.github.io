---
aliases: 
tags: 
description:
title: fat arrows (=>) and slim arrows (->) in {haskell}
created: 2024-10-24T12:42:35
updated: 2024-10-24T13:18:04
---
ref: <https://learnyouahaskell.com/types-and-typeclasses>

```
=>
->
```

=>는  Class Constraint이고 ->는 Function Arrow이다. 뭔 차이냐고? 

## Function Arrow

함수의 인풋과 아웃풋을 정의하는 타입 노테이션이다. 직관적이지 않은점이 인자들과 리턴타입이 같은 `->`으로 구분된다는 것이다. 예를 들어 인자가 하나인 함수인 `reverse`의 타입은 `[a] -> [a]` 이지만, 인자를 두개 받는 `*`는 `Num a => a -> a -> a` 으로 표현된다. 즉, 인자들 또한 `->`으로 구분되고 리턴도 `->`로 구분된다는 것이다.

```haskell
ghci> :t reverse
reverse :: [a] -> [a]

ghci> :t (*)
(*) :: Num a => a -> a -> a
```

function arrow는 방향이 반대도 가능하다. list comprehension 예제에서 확인할 수 있다.

```haskell
ghci> removeNonUppercase st = [ c | c <- st, c `elem` ['A'..'Z']]
ghci> removeNonUppercase "aAbBcC"
"ABC"
```

## Class Constraint

equality function \=\= 의 타입은 어떻게 생겼을까?

```haskell
ghci> :t (==)
(==) :: Eq a => a -> a -> Bool
```

어떤 타입 `a`한테 제약조건을 걸었다. 바로 `Eq`라는 전제이다. `Eq`는 typeclass로 , 타입의 동등성을 정의한다. 즉, `a` 인스턴스에는 반드시 동등성과 비동등성 (\=\=, \/\=)가 정의되어 있어야 한다는 뜻이다. 일반화 프로그래밍에서 볼 수 있는 전략이 여기에서도 보이다니..!

Class Constraint는 오른쪽에 따라오는 타입인자 (type variables)이 해당 클래스의 멤버여야 한다는 제약조건을 부여해줄 수 있다.
