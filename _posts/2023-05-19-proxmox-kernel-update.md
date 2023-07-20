---
layout: post
title: "Proxmox: Kernel panic"
---

We all know this, well not all, but some of us and it makes IT fun. Not!\
You just run a quick update on your hypervisor and after a reboot - nothing.\
No ping, no system.\

## Digging around

- System reboot\
  Same shit
- Rescue console\
  Nothing what I can see on first glanze, but after mounting the root partition I realised that /boot was at 100%. Of course 
  this is a Ubuntu and I had this problem on my work already.
- Hetzner vKVM\
  ![Push Lock](/assets/pix/PVE_KernelPanic.png)\
  Quod erat demonstrandum!
- KVM\
  Send support to get direct KVM console hooked up to my server.\
  Now we can reboot and choose an older kernel in our case: pve-kernel-5.15.107-1-pve


## Fix

The fix consists out of 2 parts. Regain space and reinstall the kernel.\
Let's get it on!

1. What kernel is currently running?\
This is the one we don't want to remove!\
```bash
uname -r
```
2. Check the kernels that are currently installed\
```bash
dpkg --list '*pve' | grep ^ii
```
3. Remove \
Removing kernels we don't need. Keep the one you are currently on, so you have an option back into your sys.\
```bash
sudo apt-get remove linux-image-VERSION
```
4. Autoremove\
We really need to get rid of the stuff we marked as removed so we run autoremove to get rid of the kernels\
```bash
sudo apt-get autoremove
```
5. Update GRUB\
We need to update grub, to get rid of the menu entries for the previous installed kernels:\
```bash
`sudo update-grub
```
6. Check /boot\
See if we have some space free to reinstall the broken kernel\
```bash
df -h
```
7. Reinstall the broken
```bash
sudo apt-get install --reinstall pve-kernel-5.15.107-2-pve  (replace the kernel version)
```


Reboot and fingers crossed.