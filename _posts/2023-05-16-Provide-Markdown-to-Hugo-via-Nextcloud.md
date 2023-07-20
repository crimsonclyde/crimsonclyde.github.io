---
layout: post
title: "Provide Markdown to Hugo via Nextcloud"
---

Brief info how to use Nextcloud as base and serve the Markdown documents to Hugo via Docker.

## SSHFS

Install SSHFS on the docker host!

```
sudo apt install sshfs 
```

Now mount the Nextcloud data folder to a directory

```
sudo mkdir /mnt/nextcloud/userA
sudo sshfs -o allow_other sshuser@nextcloudvm:/opt/nextcloud/data/USER/files/Hugo /mnt/nextcloud/userA
```

## Docker-Compose

We now inject the mounted folder as persistent content to our Hugo installation. All Markdown documents added to Nextcloud "Hugo" directory, which you need to create first, are therefore mounted in /src/content/posts/

```
volumes:
  - pathtoyoursrc:/src
  - /mnt/nextcloud/userA:/src/content/posts
```

## Hugo rebuild

Considering you need to change or update your markdown for design or typos a scheduled task or cron might be not suitable, therefore we need another functionality to make it easy to update Hugo. There are a lot of ways to do that and they havily depend on the access level you need:\

A) Are you the only one using it as admin?\
B) Are other users involved?\
C) Webhok (Security)\
D) Docker API expose (Security)\
E) Ansible Playbook

Right now a ansible playbook is used with a trigger to use the Master Control Program (MCP Script) to execute the "hugo" command inside the container.

```ansible
---
- name: Execute Hugo via Docker on Docker Server
  hosts: docker_server
  user: clyde
  gather_facts: False

  tasks:
    - name: Execute Hugo Command in Docker Container
      ansible.builtin.command:
        cmd: /usr/bin/docker exec clyde-hugo hugo
      become: no 
      become_method: sudo
```

The Ansible hosts needs SSH access to the docker host with SSH Key-Based authentication.