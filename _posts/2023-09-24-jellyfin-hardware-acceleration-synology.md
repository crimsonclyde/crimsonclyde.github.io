---
layout: post
title: "I♥Tech: Run Jellyfin on a Synology with hardware accerlatation"
categories: I♥Tech
---

## Exordium

Jellyfin is basically a Netflix/AmzonPrime/Disney a-like user interface for use of your own DVD/Blueray backups. Therefore
According to [Wikipedia](https://en.wikipedia.org/wiki/Jellyfin)
>Jellyfin is a free and open-source media server and suite of multimedia applications designed to organize, manage, and share digital media files to networked devices. Jellyfin consists of a server application installed on a machine running Microsoft Windows, macOS, Linux or in a Docker container, and another application running on a client device such as a smartphone, tablet, smart TV, streaming media player, game console or in a web browser


![Jellyfin](/assets/pix/Jellyfin_Synology.png)

## Prepare

Check if your model is capable of hardware acceleration. Use [PLEX spreadsheet](https://docs.google.com/spreadsheets/d/1MfYoJkiwSqCXg8cm5-Ac4oOLPRtCkgUxU0jdj3tmMPc/edit?pli=1#gid=1274624273) for this.

## Docker-Compose

I would highly recommend to use Portainer on your Synology. It is pretty easy to spin up and you don't need to rely on the limited Docker functionality served by Synology. Running something with host network, with root and priviledged mode is from a security perspective a nightmare. Consider this before you implement it!

```docker
version: '3'

services:
  jellyfin:
    image: jellyfin/jellyfin
    container_name: jellyfin
    network_mode: host
    restart: on-failure:5
    privileged: true
    devices:
      - /dev/dri/renderD128:/dev/dri/renderD128
      - /dev/dri/card0:/dev/dri/card0
    environment:
      TZ: EUROPE/WhatEverCity  # Edit this
      user: 0:0                # 0:0 = root/root
    volumes:
      - /volume1/docker/jellyfin/config:/config
      - /volume1/docker/jellyfin/cache:/cache
      - /volume1/multimedia/movies:/media/movies # Edit this based on your needs
      - /volume1/multimedia/series:/media/series # Edit this based on your needs
```

## JelliFin Settings

- Dashboard
- Playback

```
Intel QuickSync (QSV)

Enable hardware decoding for:
H264
HEVC
MPEG2
VC1
VP8
VP9
HEVC 10bit
VP9 10bit

Prefer OS native DXVA or VA-API hardware decoders
Enable hardware encoding
Allow encoding in HEVC format
Enable VPP Tone mapping
```

## F.A.Q.
*You get a error message while starting a movie (transcoding enabled).*
Try on your host:

```bash
sudo chmod 777 /dev/dri/*
```

I didn't fully get the problem, but it seems to be that Synology has some restrictions, which you don't face if you run docker on a standard linux system.
