---
description:
aliases: 
tags: 
created: 2023-04-07T01:02:02
updated: 2023-07-15T21:33:04
title: idiomatic way to output to stdout when CP in rust
---
만약 출력해야 할 양이 많은 경우, 어느정도 상한이 정해져 있는 경우 [`BufWriter`](https://doc.rust-lang.org/std/io/struct.BufWriter.html)를 사용해 볼 수 있다.

```rust
let mut output = io::BufWriter::with_capacity(CAPACITY, io::stdout());

write!(output, "{}\n"i)?;
```

[이 사람](https://www.acmicpc.net/source/55270815) 코드를 보아하니, 별도의 아웃풋 버퍼에 집어넣는게 아니라 걍 문자열에다 처박아 넣기도 한다. `output`에 계속 append가 이루어지는 걸로 보면, 정확히 어느정도 크기의 버퍼인지 알 수 없는 경우 이렇게 써도 뭐 나쁘지 않을 것 같다. 물론 버퍼 플러쉬가 일어나지 않아 무한히 많은 데이터를 출력할 땐 문제가 생기겠지만...

```rust
let mut output = String::new();
for _ in 0..t {
	let n = input();
	writeln!(output, "{} {}", fibonacci[n].0, fibonacci[n].1).unwrap();
}
println!("{output}");
```
