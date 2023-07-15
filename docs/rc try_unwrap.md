---
description:
aliases: 
tags: 
created: 2023-04-02T06:11:08
updated: 2023-07-15T21:33:03
title: rc try_unwrap
---
- https://docs.rs/rc/latest/rc/fn.try_unwrap.html?search=try_unwrap
- unwraps the contained value if the `Rc<T>` is unique.
- otherwise, `Err` is returned with the same `Rc<T>`
