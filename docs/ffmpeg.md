---
aliases: 
tags: 
description:
title: ffmpeg
created: 2024-03-22T21:58:34
updated: 2024-03-25T21:33:09
---

## 가장 좋은 코덱은?

[stackexchange / how-can-i-reduce-a-videos-size-with-ffmpeg](https://unix.stackexchange.com/questions/28803/how-can-i-reduce-a-videos-size-with-ffmpeg)

1. 코덱을 좋은 걸로 바꾼다. H.265
2. `-crf` 값을 **높혀** 비트레이트를 **낮춘다** ~~이렇게 비직관적일 수가~~

```
ffmpeg -i input.mp4 -vcodec libx265 -crf 28 output.mp4
```

## frame rate를 수정하려면?

<https://trac.ffmpeg.org/wiki/ChangingFrameRate>

```
ffmpeg -i <input> -filter:v fps=30 <output>
```

## resolution을 변경하려면?

<https://ottverse.com/change-resolution-resize-scale-video-using-ffmpeg/>

```
ffmpeg -i input.mp4 -vf scale=$w:$h <encoding-parameters> output.mp4
```
