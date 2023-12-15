---
layout: post
title: "I♥Tech: HP MicroServer Gen10: Unraid freezes"
categories: I♥Tech
---

Tech Troubles Transformed: My Odyssey with Anubis, the Unyielding Server or why the title and categorie should be: I☠️Tech instead of love

Recently, I embarked on a challenging journey with Anubis, my new server powered by Unraid. This was no love story; instead, it was a tale of tech trials and tribulations. Picture this: Anubis, becoming so unresponsive every 1-4 days (sometimes just hours!), that a hard power-down was my only recourse.

My quest for solutions was exhaustive. Picture me, scouring the internet's depths for hours, hunting for anyone who'd faced this tech demon. I went full detective mode: disabling features, ejecting suspicious hardware, and even connecting a monitor in hopes of catching a glimpse of a kernel panic or any clue. But alas, nothing. The server crashed relentlessly, impervious to keystrokes, BIOS tweaks (like enabling IOMMU and disabling NIC stacking), and my growing frustration.

Imagine the scene: an endless cycle of hard stops and restarts, each time rolling the dice against data loss or worse, a non-rebootable system. Fortune favored me, though, and I narrowly escaped these digital disasters.

Just when I was about to wave the white flag, the villain revealed itself. Like a plot twist in a tech thriller, it was the Docker network all along. Switching from the default "macvlan" to "ipvlan" was my eureka moment.

The Takeaway for Fellow Tech Warriors:

If you're in the trenches with a HP MicroServer Gen10, sporting an AMD Opteron™ X3421 and a Broadcom Network card, here's my hard-earned wisdom:

- In Unraid, navigate to Settings -> Docker and change the Docker custom network type to 'ipvlan'.
- Dive into the HP MicroServer BIOS and enable IOMMU.

Was the network type the root cause? Can't say for sure. But enabling IOMMU cleared a pesky GPU error at startup, proving that banishing errors, like vanquishing foes, is always a victory.

So, here's to tech battles fiercely fought and smartly won. Part of ship, part of crew, yo-ho!

[Unraid Thread](https://forums.unraid.net/topic/148825-hp-microserver-gen10-freezes)