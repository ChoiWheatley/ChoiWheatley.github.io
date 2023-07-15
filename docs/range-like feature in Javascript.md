---
description:
aliases: 
tags: 
created: 2023-05-23T15:27:29
updated: 2023-07-15T21:33:03
title: range-like feature in Javascript
---
- https://stackoverflow.com/a/10050831

```javascript
console.log([...Array(5).keys()]);

for (const x of Array(5).keys()) {
	console.log(x);
}
```
