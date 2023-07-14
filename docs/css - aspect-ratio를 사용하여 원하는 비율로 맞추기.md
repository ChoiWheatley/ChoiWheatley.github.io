---
description:
aliases: 
tags: 
created: 2023-05-19T17:35:46
updated: 2023-07-11T15:21:09
title: css - aspect-ratio를 사용하여 원하는 비율로 맞추기
---
- https://www.w3schools.com/howto/howto_css_aspect_ratio.asp

```css
.container {
	aspect-ratio: 3 / 2;
}
```

- https://stackoverflow.com/questions/12912048/how-to-maintain-aspect-ratio-using-html-img-tag

만약, 비율을 맞췄는데 이미지가 구겨진다? => `object-fit: contain`을 사용하여 크롭되게 만들자