---
layout: post
title: "I♥Tech: Portainer Upgrade (Synology)"
categories: I♥Tech
---

## Upgrade Portainer

To upgrade Portainer on a Synology NAS. This is a quick and dirty approach.

### Steps

1. Login via SSH  

```shell
ssh youruser@yournasip
```

1. Update Portainer (Community Edition)

```bash
sudo -i
docker stop portainer
docker rm portainer
docker pull portainer/portainer:latest
docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/v
ar/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```

1. Login  
This might take 1 minute, set a passwort wait until the local environment is loaded