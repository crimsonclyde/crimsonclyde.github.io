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

Yes, well this post holds some clues, but there is more!

### Unable to login (SDDM)

You try to login but nothing happens?  
You enter your password and then nothing happens, screen freezes and that's it?  
Well, trust me you are not alone, and that not Lenovo T470s only that seems to affect a lot of people out there.

Seems it is a bug in Breeze or in the garbage collector. Discussion is still ongoing.  
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

I am darn sure its one of the options, but I haven't sorted it out entirely.  
This should present you the full picture.

**Update**

Seems like it's just "linux-firmware".  
Removed all lines in iwlwifi.conf - reboot - still works and dmesg reports now:

```text
[    6.394413] iwlwifi 0000:3a:00.0: Detected crf-id 0xbadcafe, cnv-id 0x10 wfpm id 0x80000000
[    6.394546] iwlwifi 0000:3a:00.0: PCI dev 24fd/1010, rev=0x230, rfid=0xd55555d5
[    6.394551] iwlwifi 0000:3a:00.0: Detected Intel(R) Dual Band Wireless AC 8265
[    6.431801] iwlwifi 0000:3a:00.0: loaded firmware version 36.ca7b901d.0 8265-36.ucode op_mode iwlmvm
[    6.737882] iwlwifi 0000:3a:00.0: base HW address: 0c:54:15:35:af:d4, OTP minor version: 0x4
[    7.607490] iwlwifi 0000:3a:00.0: Registered PHC clock: iwlwifi-PTP, with index: 1
```

I still recommend to install fwupdmgr.

### SDDM

Well, easy, just use another theme:

- Settings -> Colors & Themes -> Login Screen (SDDM)
- Change to another theme which is not Breeze


#### Login primary screen

You should be able to login with your primary screen, if it is a laptop, just disconnect all screens and try to use the laptops main screen to login.

#### Cannot login on any screen

If you still cannot login:


- Switch to another TTY session (text) 
  CTRL+ALT and some Fx key > 2
- Login with your user and pass
- Open SDDM config
  `sudo nano /etc/sddm.conf.d/kde_settings.conf`
- Remove the line "Current=Breeze"
- Restart SDDM
  `sudo systemctl restart sddm.service`