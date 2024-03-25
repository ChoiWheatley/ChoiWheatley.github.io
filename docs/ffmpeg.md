---
aliases: 
tags: 
description:
title: ffmpeg
created: 2024-03-22T21:58:34
updated: 2024-03-22T23:05:37
---

## 가장 좋은 코덱은?

[stackexchange / how-can-i-reduce-a-videos-size-with-ffmpeg](https://unix.stackexchange.com/questions/28803/how-can-i-reduce-a-videos-size-with-ffmpeg)

```
ffmpeg -i input.mp4 -vcodec libx265 -crf 28 output.mp4
```

## frame rate를 수정하려면?

<https://trac.ffmpeg.org/wiki/ChangingFrameRate>

```
ffmpeg -i <input> -filter:v fps=30 <output>
```
