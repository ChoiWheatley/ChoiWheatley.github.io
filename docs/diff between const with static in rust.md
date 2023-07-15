---
aliases: 
tags: 
description:
title: diff between const with static in rust
created: 2023-03-26T21:47:35
updated: 2023-07-15T21:30:20
---
In Rust, `const` and `static` are both used to define constants. [The difference between them is that `const` is a compile-time constant while `static` is a runtime constant](https://stackoverflow.com/questions/2216239/what-is-the-difference-between-a-static-and-const-variable)[1](https://stackoverflow.com/questions/2216239/what-is-the-difference-between-a-static-and-const-variable).

[A `const` is a value that can be computed at compile time and is stored wherever the constant is used](https://web.mit.edu/rust-lang_v1.25/arch/amd64_ubuntu1404/share/doc/rust/html/book/first-edition/const-and-static.html)[2](https://web.mit.edu/rust-lang_v1.25/arch/amd64_ubuntu1404/share/doc/rust/html/book/first-edition/const-and-static.html). [It has no fixed address in memory and is inlined to each place that uses it](https://web.mit.edu/rust-lang_v1.25/arch/amd64_ubuntu1404/share/doc/rust/html/book/first-edition/const-and-static.html)[2](https://web.mit.edu/rust-lang_v1.25/arch/amd64_ubuntu1404/share/doc/rust/html/book/first-edition/const-and-static.html).

[On the other hand, a `static` variable means that the object’s lifetime is the entire execution of the program and its value is initialized only once before program startup](https://stackoverflow.com/questions/2216239/what-is-the-difference-between-a-static-and-const-variable)[1](https://stackoverflow.com/questions/2216239/what-is-the-difference-between-a-static-and-const-variable). [It has a fixed address in memory](https://stackoverflow.com/questions/52751597/what-is-the-difference-between-a-constant-and-a-static-variable-and-which-should)[3](https://stackoverflow.com/questions/52751597/what-is-the-difference-between-a-constant-and-a-static-variable-and-which-should) and can be changed at runtime[4](https://stackoverflow.com/questions/30516292/difference-between-static-and-const-variables).
