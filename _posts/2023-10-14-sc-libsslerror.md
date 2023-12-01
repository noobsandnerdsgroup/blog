---
title: "Sven Co-op How to fix libssl1.0.0:i386 errors on Ubuntu"
author: riza
date: 2023-10-14
categories: [Games]
tags: [Sven Co-op, Guides]
---

This guide was made specifically for Ubuntu 22.04. It should also work with Debian however you will need to find packages yourself as we do not link debian packages here. This guide is for both SC Clients and SCDS. IMPORTANT NOTE: DO NOT INSTALL THE .deb FILE, IT WILL MESS WITH YOUR SYSTEM UPDATES WHICH IS A SECURITY RISK! Hopefully in the next Sven Coop update this should be fixed since they will be shipping these files in the game files themselves. If you have any questions please ask in the #tech-support channel on the official [Sven Coop Discord Server](https://discord.gg/svencoop).

1. Read through the paragraph above carefully, we are not responsible for you breaking your system or you misinterpreting what this guide is for. 
2. Double check just to be sure, we are serious about installing the .deb file breaking stuff.
3. Download the files need via your web browser and the link below. 
[https://packages.ubuntu.com/focal/i386/libssl1.1/download](https://packages.ubuntu.com/focal/i386/libssl1.1/download)
4. Extract the files to a location you can easily access. You should be left with a folder called usr, be careful to not extract it to a location where this can cause issues.
5. Follow this file path to find the files we need 
```
    usr/lib/i386-linux-gnu
```    
 They should be called libcrypto.so.1.1 and libssl.so.1.1 
6. In a file manager open your SCDS root directory. If you are using a VPS or something similar then I expect you to know how to access this already. 
7. Drop the needed files into your SCDS root directory.

Attempt to launch SC or SCDS the same way you did before following this guide. If you have a new error then it's a step in the right direction and you should get in contact with people on the [official sven coop discord server](https://discord.gg/svencoop). If you have no errors now then great! You should now be able to connect to your server or launch your game now.
