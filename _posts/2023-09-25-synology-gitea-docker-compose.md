---
layout: post
title: "I♥Tech: Run Gitea on a Synology"
categories: I♥Tech
---

## Exordium

I really love GitLab - but for home usage, it is simply to bloated. So Gitea should do the trick hopefully. Here is a working container.

## Pre-Requirements

Create folders change permissions.

1. Log in to your Synology via SSH
2. Run

```bash
sudo mkdir /volume1/docker/gitea
sudo mkdir /volume1/docker/gitea/db
sudo mkdir /volume1/docker/gitea/data
id                   # Note down: typical it is something likeuid=1026(uruser) gid=100(users)
sudo chown uruser:users -R /volume1/docker/gitea
sudo chmod 771 -R /volume1/docker/gitea
```

3. Check /etc/group
I added uruser to users group, because I had some problems with write permissions.

## Docker Compose File

```docker
version: "3.9"
services:
  db:
    image: postgres
    container_name: gitea-db
    hostname: gitea-db
    security_opt:
      - no-new-privileges:true
    healthcheck:
      test: ["CMD", "pg_isready", "-q", "-d", "gitea", "-U", "gitea"]
      timeout: 45s
      interval: 10s
      retries: 10
    user: 1026:100
    volumes:
      - /volume1/docker/gitea/db:/var/lib/postgresql/data:rw
    environment:
      - POSTGRES_DB=gitea
      - POSTGRES_USER=gitea
      - POSTGRES_PASSWORD=giteapass   # Change!
    restart: on-failure:5

  gitea:
    image: gitea/gitea:latest
    container_name: gitea
    hostname: gitea
    security_opt:
      - no-new-privileges:true
    healthcheck:
      test: wget --no-verbose --tries=1 --spider http://localhost:3000/ || exit 1
    ports:
      - 3000:3000
      - 222:22
    volumes:
      - /volume1/docker/gitea/data:/data
      - /etc/TZ:/etc/TZ:ro
      - /etc/localtime:/etc/localtime:ro
    environment:
      - USER_UID=1026      # This shall be the same ID you got in pre-requirements from id command
      - USER_GID=100       # This shall the same group you got in pre-requirements from id command
      - GITEA__database__DB_TYPE=postgres
      - GITEA__database__HOST=gitea-db:5432
      - GITEA__database__NAME=gitea
      - GITEA__database__USER=gitea
      - GITEA__database__PASSWD=giteapass    # Change!
    restart: on-failure:5
```

## Setup

Wait until all containers are up. The DB takes some time, check the logs of the Postgres DB if it's running properly. After that fire up your browser and go to your IP:3000 and run through the installer. Takes about 5 minutes until it's done.
