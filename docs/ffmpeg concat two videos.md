---
aliases: 
tags: 
description:
title: ffmpeg concat two videos
created: 2024-04-02T18:12:46
updated: 2024-04-02T18:28:40
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

---

## FOR MP4 FILES

For **.mp4** files (which I obtained from DailyMotion.com: a 50 minute tv episode, downloadable only in three parts, as three .mp4 video files) the following was an effective solution for **Windows 7**, and does **NOT** involve re-encoding the files.

I renamed the files (as *file1.mp4*, *file2.mp4*, *file3.mp4*) such that the parts were in the correct order for viewing the complete tv episode.

Then I created a simple **batch file** (concat.bat), with the following contents:

```
:: Create File List
echo file file1.mp4 >  mylist.txt 
echo file file2.mp4 >> mylist.txt
echo file file3.mp4 >> mylist.txt

:: Concatenate Files
ffmpeg -f concat -i mylist.txt -c copy output.mp4
```

The **batch file**, and **ffmpeg.exe**, must both be put in the same folder as the **.mp4 files** to be joined. Then run the batch file. It will typically take less than ten seconds to run.  
. 

**Addendum** (2018/10/21) -

If what you were looking for is a method for specifying all the mp4 files in the current folder without a lot of retyping, try this in your Windows **batch file**instead (**MUST** include the option **-safe 0**):

```
:: Create File List
for %%i in (*.mp4) do echo file '%%i'>> mylist.txt

:: Concatenate Files
ffmpeg -f concat -safe 0 -i mylist.txt -c copy output.mp4
```

This works on Windows 7, in a batch file. **Don't** try using it on the command line, because it only works in a batch file!
