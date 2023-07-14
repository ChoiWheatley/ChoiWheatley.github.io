---
description:
aliases: 
tags: 
created: 2023-05-29T17:37:49
updated: 2023-07-11T15:21:09
title: CCBV, Classy Class Based View -- django
---
- https://ccbv.co.uk/ 
- 모든 장고 클래스 기반 View들을 모아놓은 신기한 사이트이다. 치트시트처럼 모아놓은 것 같은뎅

## What are class-based views anyway?

Django's class-based generic views provide abstract classes implementing common web development tasks. These are very powerful, and heavily-utilise Python's object orientation and multiple inheritance in order to be extensible. This means they're more than just a couple of generic shortcuts — they provide utilities which can be mixed into the much more complex views that you write yourself.

## Great! So what's the problem?

All of this power comes at the expense of simplicity. For example, trying to work out exactly which method you need to customise, and what its keyword arguments are, on your `UpdateView` can feel a little like wading through spaghetti — it has 10 separate ancestors (plus `object`), spread across 3 different python files. This site shows you exactly what you need to know.

## How does this site help?

To make things easier, we've taken all the attributes and methods that every view defines or inherits, and flattened all that information onto one comprehensive page per view. Check out [UpdateView](https://ccbv.co.uk/UpdateView/), for example.