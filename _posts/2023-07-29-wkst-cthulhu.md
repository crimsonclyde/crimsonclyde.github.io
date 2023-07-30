---
layout: post
title: "I♥Tech: New Workstation"
categories: I♥Tech
---

# Cthulhu 

Behold nightmares of evil - raw power against brute forces of hell.  
Lilith behold! I got myself finally a new machine!

For those interested, these are the specs and the prices for a decent workstation currently:

| Item| Number | Price|
|---|---|---|
| be quiet! PURE BASE 500DX Window, Tower-Gehäuse schwarz, Window-Kit | 1596912 | 87,90€ |
| be quiet! Dark Rock 4, CPU-Kühler schwarz, Single-Tower | 1441525 | 69,90€ |
| Thermaltake Toughpower GF2 ARGB 750W, PC-Netzteil schwarz, 4x PCIe, Kabel-Management, 750 Watt | 1704321 | 119,90€ |
| ASUS PRIME B650M-A WIFI II, Mainboard schwarz | 1903987 | 189,90€ |
| AMD Ryzen™ 7 7800X3D, Prozessor Boxed-Version | 1898270 | 459,00€ |
| Gainward GeForce RTX 4070 Ghost OC, Grafikkarte DLSS 3, 3x DisplayPort, 1x HDMI 2.1 | 1905049 | 599,00€ |
| Corsair DIMM 32 GB DDR5-5600 (2x 16 GB) Dual-Kit, Arbeitsspeicher schwarz, CMK32GX5M2B5600C36, Vengeance, INTEL XMP | 1806738 | 94,90€ |
| SAMSUNG 980 PRO 2 TB, SSD PCIe 4.0 x4, NVMe 1.3c, M.2 2280, intern | 1706158 | 129,90€ |

![Cthulhu](/assets/pix/Wkst_Cthulhu.JPG)

*Pro Tip:*  
If your setup a Windows 11 nowaday I would highly recommend to use `winget` aka App Installer.  
If not already available on your system use the link below.  

[App Installer](https://apps.microsoft.com/store/detail/appinstaller/9NBLGGH4NNS1)

Now you can use this like `apt` on Debian or `brew` on macOS.

```powershell
winget search firefox               # Search function if a package is available
winget install Mozilla.Firefox      # Install with ID
winget update --all                 # Update all apps

winget
The following commands are available:
  install    Installs the given package
  show       Shows information about a package
  source     Manage sources of packages
  search     Find and show basic info of packages
  list       Display installed packages
  upgrade    Shows and performs available upgrades
  uninstall  Uninstalls the given package
  hash       Helper to hash installer files
  validate   Validates a manifest file
  settings   Open settings or set administrator settings
  features   Shows the status of experimental features
  export     Exports a list of the installed packages
  import     Installs all the packages in a file
  pin        Manage package pins
```

Installing it the old fashioned way is so 2020! This is not a new feature, I use it already for quite a few years and nowadys it is a solid piece of program.