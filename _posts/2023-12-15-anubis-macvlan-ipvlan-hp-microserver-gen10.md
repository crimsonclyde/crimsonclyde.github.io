---
layout: post
title: "I♥Tech: HP MicroServer Gen10: Unraid freezes"
categories: I♥Tech
---

One more time this should be more I☠️Tech instead of love.

Recently I had hard times with Anubis, my new server with Unraid as operating system. After 1-4 days, sometimes after hours, the system ended in an unresponsive state. In fact, so unresponsive, that I had to use the power button to shut it hard down.

I tried many things. Searching the internet for hours for someone with the same problem. Switched of most of the functionalities. Removed hardware which might be problematic and connected a monitor. Just in hope to see something, like a kernel panic or anything. Nothing. The server crashed. And a key stroke an attached keyboard did nothing. BIOS settings: enabling IOMMU and NIC stacking disabled. Nope. Smame shit.

Always and endlessly hard stop and starting all again, hoping to not lose data or having a system which does not start again. I had luck, a lot of.

After mostly giving up, I found the culprit. Like a lot of others it seems to be the problem with Docker network. I switched from "macvlan" which is default to "ipvlan" and that's mostly it.


Conclusion:
If you own a HP MicroServer Gen10 with an AMD Opteron™ X3421 and a Broadcom Network card try:

1. Unraid: Settings -> Docker -> Docker custom network type: ipvlan
2. HP MicroServer BIOS -> Enable IOMMU

I am not entirly sure if the problem was the custom network type, but enabling IOMMU in the BIOS removed one of the errors with the GPU during start up. Never a bad thing to get rid of errors, right? 

Good fight - good night

[Unraid Thread](https://forums.unraid.net/topic/148825-hp-microserver-gen10-freezes)