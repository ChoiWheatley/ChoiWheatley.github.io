---
aliases: 
tags: 
description:
title: ffmpeg
created: 2024-03-22T21:58:34
updated: 2024-05-14T13:18:19
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

## 두 영상을 합치려면?

[[ffmpeg concat two videos]]

## 영상 일부분을 잘라내려면?

[stackoverflow / cutting multimedia files](https://stackoverflow.com/questions/18444194/cutting-multimedia-files-based-on-start-and-end-time-using-ffmpeg)

```
ffmpeg -ss 00:01:00 -to 00:02:00 -i input.mp4 -c copy output.mp4
```
