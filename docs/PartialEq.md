---
description:
aliases: 
tags: 
created: 2023-03-14T00:19:29
updated: 2023-07-15T21:33:03
title: PartialEq
---

> This trait allows for partial equality, for types that do not have a full equivalence relation. For example, in floating point numbers `NaN != NaN`,so floating point types implement `PartialEq` but not [`trait@Eq`].Formally speaking, when `Rhs == Self`, this trait corresponds to a [partial equivalence relation](https://en.wikipedia.org/wiki/Partial_equivalence_relation).
