---
description:
aliases: 
tags: useful
created: 2023-03-30T22:07:38
updated: 2023-07-15T21:33:04
title: fastest idiomatic io routine in rust for programming contests
---
- [src](https://stackoverflow.com/questions/56449027/fastest-idiomatic-i-o-routine-in-rust-for-programming-contests)

```rust
use std::io::{self, prelude::*};

fn solve<B: BufRead, W: Write>(mut scan: Scanner<B>, mut w: W) {
    let n = scan.token();
    let mut a = Vec::with_capacity(n);
    let mut b = Vec::with_capacity(n);
    for _ in 0..n {
        a.push(scan.token::<i64>());
        b.push(scan.token::<i64>());
    }
    let mut order: Vec<_> = (0..n).collect();
    order.sort_by_key(|&i| b[i] - a[i]);
    let ans: i64 = order
        .into_iter()
        .enumerate()
        .map(|(i, x)| a[x] * i as i64 + b[x] * (n - 1 - i) as i64)
        .sum();
    writeln!(w, "{}", ans);
}

fn main() {
    let stdin = io::stdin();
    let stdout = io::stdout();
    let reader = Scanner::new(stdin.lock());
    let writer = io::BufWriter::new(stdout.lock());
    solve(reader, writer);
}

pub struct Scanner<B> {
    reader: B,
    buf_str: String,
    buf_iter: std::str::SplitWhitespace<'static>,
}
impl<B: BufRead> Scanner<B> {
    pub fn new(reader: B) -> Self {
        Self {
            reader,
            buf_str: String::new(),
            buf_iter: "".split_whitespace(),
        }
    }
    pub fn token<T: std::str::FromStr>(&mut self) -> T {
        loop {
            if let Some(token) = self.buf_iter.next() {
                return token.parse().ok().expect("Failed parse");
            }
            self.buf_str.clear();
            self.reader
                .read_line(&mut self.buf_str)
                .expect("Failed read");
            self.buf_iter = unsafe { std::mem::transmute(self.buf_str.split_whitespace()) };
        }
    }
}
```

- [백준 고인물의 코드](https://www.acmicpc.net/source/35458381)
	- 특별히 io 속도가 빠를 것처럼 보이지 않는뎅? 다만 `read` 메서드를 제너릭하게 구현함으로써 원하는 타입을 지정만 하면 마치 `cin`처럼 읽을 수 있게 했다는 점이 얻어갈 만 하다.

```rust
/* FAST IO */ 
use std::io::{Read, Write}; 
trait _P { fn read<T: std::str::FromStr>(&mut self) -> T; } 
impl<'a> _P for std::str::SplitWhitespace<'a> { 
	fn read<T: std::str::FromStr> (&mut self) -> T { 
		self.next().expect("EOF").parse().ok().unwrap() 
	}
}
/// ...
fn main() {
    /* FAST IO */ 
    let stdout = std::io::stdout(); 
    let mut out = std::io::BufWriter::new(stdout.lock()); 
    let mut buf = String::new(); 
    std::io::stdin().read_to_string(&mut buf).unwrap(); 
    let mut input = buf.split_whitespace();

    let n: usize = input.read();
    let root: usize = input.read();
    let queries: usize = input.read();
    // ...
}

```
