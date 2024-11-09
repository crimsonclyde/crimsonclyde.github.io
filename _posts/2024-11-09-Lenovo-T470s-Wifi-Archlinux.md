---
layout: post
title: "I♥Tech: Lenovo T470s and Arch"
categories: I♥Tech
---

## Introduction

### Probs with WiFi not working?

Always getting discconnects and running `dmesg | grep iwlwifi` like


```text
[    5.442828] ieee80211 phy0: Selected rate control algorithm 'iwl-mvm-rs'
[    6.203085] iwlwifi 0000:3a:00.0: Registered PHC clock: iwlwifi-PTP, with index: 1
[  437.551591] iwlwifi 0000:3a:00.0: Queue 10 is active on fifo 1 and stuck for 10000 ms. SW [197, 253] HW [197, 253] FH TRB=0x0c010a0d4
[  437.552750] iwlwifi 0000:3a:00.0: Microcode SW error detected.  Restarting 0x2000000.
```

Yes, well this post holds some clues, but there is more

### Probs that you cannot login with an attached monitor to your KDE Plasma6 with Breeze Theme?

You try to login but nothing happens?
There is a bug in Breeze or in the garbage collector. Discussion is still ongoing.
Jump keep on reading.

## Workarouns

### WiFi

Try:

1. Connect LAN
2. Disconnect WiFi
3. Run
    ```shell
    sudo pacman -Syu
    sudo -S linux-firmware fwupd
    sudo nano /etc/modprobe.d/iwlwifi.conf
    fwupdmgr refresh
    fwupmgr get-updates
    fwupmgr update

    # Insert:
    options iwlwifi power_save=0
    options iwlwifi 11n_disable=1 swcrypto=1

    # Reboot your system
    ```

I am darn sure its one of the options, but I haven't sorted it out entirely. This is just for to give you the full picture what I have done.

### SDDM

Well, easy, just use another theme

- Settings -> Colors & Themes -> Login Screen (SDDM)
- Change to another theme which is not Breeze

There is an ogoing discussion and loads of ppl running into this problem currently. Fix is possible.
If you cannot login login with ALT+CTRL +F4 or F5 or just another TTY

- `sudo nano /etc/sddm.conf.d/kde_settings.conf`
- Remove the line "Current=Breeze"