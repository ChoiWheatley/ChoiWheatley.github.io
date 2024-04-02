---
aliases: 
tags: 
description:
title: ffmpeg concat two videos
created: 2024-04-02T18:12:46
updated: 2024-04-02T18:12:56
---

<https://stackoverflow.com/questions/7333232/how-to-concatenate-two-mp4-files-using-ffmpeg>

## 1. [concat video filter](https://ffmpeg.org/ffmpeg-filters.html#concat)

Use this method if your inputs do not have the same parameters (width, height, etc), or are not the same formats/codecs, or if you want to perform any filtering.

Note that this method performs a re-encode of all inputs. If you want to avoid the re-encode, you could re-encode just the inputs that don't match so they share the same codec and other parameters, then use the concat demuxer to avoid re-encoding everything.

```bash
ffmpeg -i opening.mkv -i episode.mkv -i ending.mkv \
-filter_complex "[0:v] [0:a] [1:v] [1:a] [2:v] [2:a] \
concat=n=3:v=1:a=1 [v] [a]" \
-map "[v]" -map "[a]" output.mkv
```

## 2. [concat demuxer](https://ffmpeg.org/ffmpeg-formats.html#concat-1)

Use this method when you want to avoid a re-encode and your format does not support file-level concatenation (most files used by general users do not support file-level concatenation).

```bash
$ cat mylist.txt
file '/path/to/file1'
file '/path/to/file2'
file '/path/to/file3'
    
$ ffmpeg -f concat -i mylist.txt -c copy output.mp4
```

*For Windows*:

```bash
(echo file 'first file.mp4' & echo file 'second file.mp4' )>list.txt
ffmpeg -safe 0 -f concat -i list.txt -c copy output.mp4
```

or

```bash
(for %i in (*.mp4) do @echo file '%i') > list.txt
ffmpeg -safe 0 -f concat -i list.txt -c copy output.mp4

```

## 3. [concat protocol](https://ffmpeg.org/ffmpeg-protocols.html#concat)

Use this method with formats that support file-level concatenation (MPEG-1, MPEG-2 PS, DV). Do *not* use with MP4.

```bash
ffmpeg -i "concat:input1|input2" -codec copy output.mkv
```

This method does not work for many formats, including MP4, due to the nature of these formats and the simplistic physical concatenation performed by this method. It's the equivalent of just raw joining the files.

---

If in doubt about which method to use, try the concat demuxer.

## Also see

- [FFmpeg FAQ: How can I join video files?](https://ffmpeg.org/faq.html#How-can-I-join-video-files_003f)
- [FFmpeg Wiki: How to concatenate (join, merge) media files](https://trac.ffmpeg.org/wiki/Concatenate)
