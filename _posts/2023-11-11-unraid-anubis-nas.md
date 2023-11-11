---
layout: post
title: "I♥Tech: Anubis Presentation"
categories: I♥Tech
---

Welcome to the next chapter ([🕰️time heist to our chapter](https://clyde.crimson.space/posts/project-anubis/)) of our digital odyssey, Project Anubis, where we transcend traditional tech boundaries! 🌌 Dive deeper into our story at Project Anubis.

🔐 Unleashing Unraid: Imagine a software universe where the shackles of conventional RAID are broken. Enter Unraid, the heart of Project Anubis. This isn't just an operating system; it's a liberator for your disks, bringing them together into a harmonious array without the rigid confines of standard RAID setups.

🌟 The Unraid Advantage: Like a cosmic dance, Unraid elegantly orchestrates your disks, offering a unique blend of flexibility and resilience. Each disk retains its individuality, yet contributes to a larger, more robust digital cosmos. Yes, every technology has its yin and yang, its strengths and weaknesses. Unraid is no exception, but its adaptability is its superpower.

🛡️ Layered Security: In the universe of Project Anubis, safeguarding our digital treasures is paramount. We believe in the power of layered security - a fortress not just built with bricks, but with wisdom and foresight. Unraid is our ally in this quest, providing a foundation upon which we build our bastions of digital protection.

🌀 Beyond Technical Jargon: We're venturing beyond the black hole of technical Tourette's, into a realm where the essence of technology is woven into the narrative of Project Anubis. It's not just about specs and codes; it's about the story, the adventure, and the vision.

Join us in this thrilling chapter of Project Anubis, where Unraid is not just a tool, but a gateway to new possibilities in the digital universe! 🌠

I try to give you the idea behind it, without (most) of the usual technic tourette.

![Unraid Dashboard](/assets/pix/Anubis_Unraid_Dashboard.png)

## The Array

We have 4 harddisk drives, each 5TB in size.  
These are in an array means we can count them without any loss to a total of 20TB to our disposal.

Great, but what happens if a disk fails? As more disks you have, as more likely it is, that a disk will fail and is there any protection? Right now, no. Which is a bad thing.

## The Arrays Security

Parity drive magic!

In the digital world of computers, data is represented using only two states: zero (0) and one (1). This binary system is the foundation of how computers process and store information.

In an Unraid system, which is a type of computer storage setup, there's a concept called a "parity disk." This is a special disk used to protect against data loss. To understand how it works, let's consider a simplified example with four disks:

    DISK1: 0
    DISK2: 1
    DISK3: 1
    DISK4: 1

We determine the overall state of these disks as either "even" or "odd," based on the number of ones (1s). In this case, there are three ones, which makes the state "odd."

The parity disk's job is to keep track of this state. Since the state is odd, the parity disk records a 1.

Now, imagine DISK2 fails and is no longer accessible. With DISK2 gone, we only have two ones left (from DISK3 and DISK4), making the state "even." This is opposite to what our parity disk says (which is odd). This discrepancy helps us understand that something has gone wrong, specifically that a disk has failed.

When we replace the failed disk with a new one, we need to restore its data. The system knows that the overall state must be "odd" (as the parity disk indicates). So, it writes a 1 to the new disk, restoring the odd state.

This example is very simplified. In reality, the data is saved in many small sections, called sectors, across all the disks, including the parity disk. This means the parity disk tracks the state of each sector across all other disks. As a result, if any single disk fails, the system can use the information on the parity disk to rebuild the lost data.

This method allows an Unraid system to survive the failure of one disk without losing any data. It's a clever way to provide data security, but it's important to note that this only protects against a single disk failure at a time.

## Docker (Container)

I rely heavily on containerization, and the built-in "apps" system on Unraid is nearly perfect for a homelab setup. Just in case I need more control, I can fire up a Portainer container, but most of the time, the built-in system is superb.

I don't want to overload you with details, but if you have questions about the services I run, feel free to get in touch. I can explain more, but that would be too much for this post. Here's a quick rundown:

- **Adminer:** Used as a PHPMyAdmin replacement, it's more lightweight. Essentially, it's a My/MariaDB database frontend.
- **Authelia:** Adds an additional layer of security to my pages.
- **Code-Server:** Yes, it's Visual Studio Code in your browser, with access to my Docker containers, making it super convenient to change things on the persistent drives.
- **Duplicati:** An additional layer of protection for the most important data on the machine; basically a backup system.
- **Gitea:** Used as a GitLab replacement. GitLab is far too bloated for my personal use, whereas Gitea fits perfectly.
- **Glances:** For system monitoring, usable via API on other systems. Quite neat, though I mostly keep it stopped as it consumes a lot of CPU. Apart from that, it's a great tool.
- **Grafana/InfluxDB/Telegraf:** A classic TIG stack which I formerly hosted on Hetzner. However, paying 60€/month just for a TIG stack I'm not currently using is a lot, so I'm trying to replicate this in my homelab.
- **Hetzner-ddns:** Updates Hetzner DNS via API for a classic dynamic DNS setup. Now, I can use a subdomain and Let's Encrypt to secure services with a TLS certificate.
- **HomeAssistant:** The new addition, replacing OpenHab3 for all IoT stuff. I gave it a test run and would never go back to OpenHab3, which I used for over 5 years. It's that good!
- **Homepage:** A dashboard for my services, displaying additional information. I've tried others; this one is okay. Homarr was good but more focused on the .arr systems.
- **Jellyfin:** Our media vault. I enjoy having a home setup similar to Prime Video or Netflix. Updating data takes time, but it's worth it.
- **MariaDB:** A necessary database system. I have some old blogs I want to revive when I find the time.
- **Minecraft:** I moved my Minecraft server from Hetzner to my local system. It's not a public server; I'm the only player.
- **Mosquitto MQTT Broker:** A lightweight publish-subscribe, machine-to-machine protocol for messaging. Mainly used for IoT projects.
- **Netdata:** A powerful surveillance system for your homelab. It offers extensive data, filters, and a nice GUI. Live monitoring is CPU intensive, but the data collected is significantly more than what Glances offers.
- **NGINX Proxy Manager (NPM):** A reverse proxy for added security on my projects exposed to the public internet. Paired with Authelia, it's quite handy. NPM manages all certificates via Let's Encrypt and is a bliss once set up.
- **Node-Red:** JavaScript in a GUI. If you need data from somewhere or want to create home automations, this is the tool to use.
- **Pi-Hole:** Of course, we block ads and anything that consumes bandwidth unnecessarily.
- **Portainer:** Manages Docker/DockerCompose in a GUI. I've used it for years and still do in my work.
- **TinyMediaManager:** Makes tagging media files easy. This program was the reason I tried Jellyfin, and I wouldn't want to be without it.

## Virtualization

Since my CPU is already heavily at work (9%-60%) with additional heavy workloads for internal backups and automisation I just use a single Ubuntu from time to time on my iPad.

## Uninterruptible power supply

If you run such a system, with TBs of data and services, you want to be prepared if the grid goes down.  
Right now that's a cheap Eaton USV Ellipse PRO 650 DIN. It is a battery and able to shut down your system if the batteries power level goes down. Seriously if you have a NAS, protect it!

## Mos Eisley Dashboard

[GetHomepage](https://gethomepage.dev)

![Homepage](/assets/pix/Anubis_Homepage_Dashboard.png)


## Price tag

Last not least, just to give you rough sum if you consider to build this on your own.  
Time for research and setup not included.

| Amount | Part | Single | Total |
|---|---|---|---|
|1|HP MicroServer||570€|
|5|Seagate Barracuda ST5000LM00 5TB HDDs| 121€ | 605€ |
|2|1 Port M.2 NVMe PCIe 3.0 X4 Riser Card| 15€ | 30€ |
|2|Samsung SSD 970 EVO Plus 2TB M.2 NVMe | 113€ | 226€ |
|1| Unraid Licence Plus (12 drives) || 89€|
|1|HP 870212-B21 MicroServer Gen10 Slim SFF Enable SATA Activation Kit||40€|
|1|Eaton USV Ellipse PRO 650 DIN||180€|
||||**1.740€**|