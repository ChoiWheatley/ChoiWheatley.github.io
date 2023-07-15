---
aliases: 
tags: 
description:
created: 2023-06-23T10:57:12
updated: 2023-07-15T21:33:04
title: map an array of objects to a dictionary {js}
---
- [dev.to](https://dev.to/devtronic/javascript-map-an-array-of-objects-to-a-dictionary-3f42)

```js
let data = [
  {id: 1, country: 'Germany', population: 83623528},
  {id: 2, country: 'Austria', population: 8975552},
  {id: 3, country: 'Switzerland', population: 8616571}
];

// First Try
let dictionary = Object.assign({}, ...data.map((x) => ({[x.id]: x.country})));

// Second Try
let dictionary = Object.fromEntries(data.map(x => [x.id, x.country]));
```

result:

```js
{1: "Germany", 2: "Austria", 3: "Switzerland"}
```
