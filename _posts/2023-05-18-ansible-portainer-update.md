---
layout: post
title: "Ansible: Portainer Update"
---

Basic script to upgrade Portainer on your Docker host.

## Pre-Requirements

1. Replace all []\
   [USER] and [YOURNETWORK] are just placeholders here!
2. SUDO\
   The user on your docker host needs sudo rights with NOPASSWD in your sudoers config!\
   `sudo visudo -f /etc/sudoers.d/[USER]`\
   And add: [USER] ALL=(ALL:ALL) NOPASSWD: ALL\
   Security: You can allow apt only if you wish, see this as a example!
3. Prune\
   The last task in this playbooks deletes all images not used. Maybe you don't want this, so remove that task.

```
---
- name: Update Portainer
  hosts: docker_server
  user: [USER]
  become: yes
  tasks:
    
    - name: Install necessary system packages
      apt: 
        name: 
          - python3-pip
          - python3-setuptools
        update_cache: yes
        state: present

    - name: Install necessary python packages
      pip:
        name:
          - requests
          - docker
          - docker-compose
        state: present

    - name: Pull latest Portainer image
      community.general.docker_image:
        name: portainer/portainer-ce:latest
        source: pull
        state: present

    - name: Check if Portainer container is running
      community.general.docker_container_info:
        name: portainer
      register: portainer_info

    - name: Stop Portainer container
      community.general.docker_container:
        name: portainer
        state: stopped
      when: portainer_info.exists

    - name: Remove Portainer container
      community.general.docker_container:
        name: portainer
        state: absent
      when: portainer_info.exists

    - name: Start new Portainer container
      community.general.docker_compose:
        project_name: portainer
        definition:
          version: '3.3'
          services:
            portainer:
              image: portainer/portainer-ce:latest
              container_name: portainer
              hostname: portainer
              command: -H unix:///var/run/docker.sock
              restart: always
              ports:
                - '9000:9000'
                - '9443:9443'
              volumes:
                - '/var/run/docker.sock:/var/run/docker.sock'
                - 'portainer_data:/data'
          volumes:
            portainer_data: {}
          networks:
            default:
              name: [YOURNETWORK]
              external: true
        state: present

    - name: Prune unused Docker images
      community.general.docker_prune:
        images: yes
...

```