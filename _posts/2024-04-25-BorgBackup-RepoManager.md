---
layout: post
title: "I♥Tech: Borg Backup, Borgmatic and Borgmatic Repository Management"
categories: I♥Tech
---

## Exordium
Files are just files and running a raid or something similar should be fine you say?  
I disagree, entirely. Files are the new paper, and you should protect at least your precious once like you would protect the one ring. I did my research and finally I settled
on [Borg](https://www.borgbackup.org/) backup. I still do not trust any file silos out there in the internet. Even if they have some pretty and easy to use solutions for you.

Borg backup has it's charme that it uses repositories and is working pretty on all platforms I use.

## Server

What I use is a StorageBox on [Hetzner](https://hetzner.de), these StorageBoxes have a limited feature set but already equipped with Borg so with some help from the [Hetzner Community](https://community.hetzner.com/tutorials/install-and-configure-borgbackup) we can start set it up

## Client

On my homelab I run a Unraid server, by default Unraid does not support Borg but we can use a Docker container called [Borgmatic by sdub](https://forums.unraid.net/topic/99218-support-borgmatic/) to get the setup up and running.

### Prerequirements

- SSH Key for the repository
  No passphrase! See the Hetzner Community how to setup your StorageBox.

### Setup (Example)

I do not cover every click in detail, but that should help you start things up!

1. Install plugin "Unassigned Devices"
2. Activate Storage Bbx Samba and SSH access feature
3. Mount your storage box: //user:user.storagebox.de/backup to "storagebox"
4. Install plugin "Borgmatic" by sdub
   User Shares: /mnt (if you want to backup more than /mnt/user)
   Borg Repo: /mnt/remotes/storagebox/folder/to/repo
5. Create file crontab.txt
   This file is located where you set it up in point 4 folder config!
   ```plaintext
   0 1,13 * * * borgmatic prune create -v 1 --stats 2>&1
   0 6    * * 3 borgmatic check -v 1 2>&1
   ```
6. Create file config.yaml
   Same folder as the crontab.
   ```yaml
    # List of source directories to backup.
    source_directories:
        - /boot
        - /mnt/user/user/folderA
        - /mnt/user/user/folderB
        - /mnt/user/cache/appdata/authelia
        - /mnt/user/cache/appdata/homepage
        - /mnt/user/cache/appdata/vaultwarden

    # Paths of local or remote repositories to backup to.
    repositories:
        - path: /mnt/borg-repository
        label: local

    encryption_passphrase: {YOURREPOPASS}

    # Retention policy for how many backups to keep.
    keep_daily: 7
    keep_weekly: 4
    keep_monthly: 6

    # List of checks to run to validate your backups.
    checks:
        - name: repository
        - name: archives
        frequency: 2 weeks

    # Custom preparation scripts to run.
    before_backup:
        - echo "Starting a backup."
    after_backup:
        - echo "Finished a backup."
    on_error:
        - echo "Error during prune/create/check."

    # Databases to dump and include in backups.
    #postgresql_databases:
    #    - name: users

    # Third-party services to notify you if backups aren't happening.
    #healthchecks:
    #    ping_url: https://hc-ping.com/be067061-cf96-4412-8eae-62b0c50d6a8c
   ```
7. Transfer SSH Keys
   SSH keys into the folder ssh_keys see point 4 or edit the container to check where your folder is.
8. Start borgmatic
9.  Log in to console
10. Initiate the repo
   ```bash
   # If your ssh key has different name
   export BORG_RSH='ssh -i /root/.ssh/{YOURKEYNAME}'
   # Init repo and set a password!
   borg init --encryption=repokey-blake2 /mnt/borg-repository
   # Check if the repo is created
   borg check /mnt/borg-repository
   ```
11. Restart Borgmatic container and check your logs
    There are plenty of things to adjust and maybe I missed some, but the log file helps a lot to fix path problems.


## Borgmatic Repository Management

I have written a small dialog script which you can copy to your container and run. It installs the package 'dialog' in the Alpine container.

![Borgmatic Repository Manager](/assets/pix/Borgmatic_Repository_Management.png)

1. Copy the content of the [script](https://gist.github.com/crimsonclyde/a8f4cd8bef8dc151b3a302b0c96be272)
2. Create a new file in the container called "borg.sh"
3. Make it executable
   `chmod +x borg.sh`
4. Run it
   `./borg.sh`