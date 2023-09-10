---
layout: post
title: "I♥Tech: The great video cleanup - Merging Videos"
categories: I♥Tech
---

## Exordium

You have a bunch of digital backups of your hard earned movie DVDs and BlueRays? You want to have a feeling like your library feels like a Netflix/Prime/Whatever? Well fear not the end is not nigh!
This will be a short series of posts. We start with the easy part!


## Merge outdated movies

We have assembled a lot of clutter during the past. Loads of different codecs and ways to keep our backup copies protected and usable. Sometimes a movie split in 3 parts to still be able to burn it back to a CD and watch it. But those times are over - we need to move on.

There is a nice trick to do that on Windows you download yourself FFMPEG and let's assume you have 2 .avi files

```powershell
ffmpeg -i "concat:input1.avi|input2.avi" -c copy output.avi
```

### m2ts

For m2ts files you need to do some more tweaks.

1. We create a file called list.txt\
The file shall contain the following, please update the filenames according to yours.\
```text
file '000000.m2ts'
file '000001.m2ts'
file '000002.m2ts'
```

2. Merge the files  
```powershell
ffmpeg -f concat -safe 0 -i list.txt -map 0 -c copy output.mkv
```
We use mkv (Matroska) file format since it supports all audio/subtitle channels instead of a simple .avi.

## Use Synology (speed up the process)
Right now my computer is connected to my WiFi which is very busy and the merging or transcoding takes a lot of time. Why not running the entire process on the NAS instead of using the network in between the storage and my computer?

1. Update FFMPEG
Let's assume you have your backups on a Synology 7 DSM (NAS). There is a way via SynoCommunity to get FFMPEG on your Synology - but you will have hard times with audio. I tried some stuff but nothing works.
The problem is the SynoCommunity's version of FFMPEG is `ffmpeg version 4.1.8 Copyright (c) 2000-2021 the FFmpeg developers` bit out of date.
A easy way to fix it, without the fear of breaking something else is to use a static-compiled binary of FFMPEG. Johan Van Sickle has a brilliant tutorial to run through and the files you need.

[FFMPEG Johan Van Sickle](https://www.johnvansickle.com/ffmpeg/faq/)

I stopped at this point "Without any further steps I can start using ffmpeg with my relative path to the binary", because I just want to use it to merge or transcode videos not to update my entire Synology DSM.
Just remember where you have downloaded the binary. Now you can run the same stuff on the Synology directly without using the network - which makes it 80 times faster.

2. Run the the binary 
```bash
/var/services/homes/youruser/ffmpeg/ffmpeg-git-20230908-amd64-static/ffmpeg -f concat -safe 0 -i list.txt -analyzeduration 2147483647 -probesize 2147483647 -map 0 -c copy output.mkv
```