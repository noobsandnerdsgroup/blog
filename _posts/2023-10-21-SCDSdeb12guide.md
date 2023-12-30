---
title: "Setting up a Sven Co-op Server"
author: riza
date: 2023-10-21
categories: [Games]
tags: [Sven Co-op, Guides, Server Hosting]
---

 ***Both the "easy" and "hard" way in one guide!***


## The Easy Way (Listen Servers)
*easy peasy lemon squeezy*

### P2P
---

This is the easiest method. It's not a dedicated server sure, but it will work if all you want is to play with friends and don't need a dedicated server. The link below will take you to a guide made by the Sven Co-op Devs. It's a really good guide that goes into great details.

[https://steamcommunity.com/sharedfiles/filedetails/?id=1705149010](https://steamcommunity.com/sharedfiles/filedetails/?id=1705149010)

The gist of it is:

```
- Find your Steam ID 
This should look like STEAM_0:0:00000000
- Start Sven Co-op
- Click Create Game and change the map to what you want then start the game
- Have your friends connect in console with this command

connect STEAM_0:0:00000000
```

>Make sure to replace the placeholder id with your own [steam id](https://www.steamidfinder.com/)

## The Hard Way (Dedicated Servers)

>This part expects that you are using Debian 12, know how to use a terminal emulator, already have steam ports open, and use strong passwords. We do not recommend hosting dedicated servers on your home network and personal computers. Instead, please look into a VPS (Virtual Private Server)

### Manually setup SCDS
---

We're not in Kansas anymore, proceed with caution.

### **Setup SteamCMD**

- Change sources.list 
```bash
sudo nano /etc/apt/sources.list
```
>GNU nano is a text editor. It comes with most Linux distros. This command opens the file sources.list located in /etc/apt/

- Paste these lines of text into the document

deb http://deb.debian.org/debian bookworm main non-free-firmware
deb http://deb.debian.org/debian bookworm-updates main non-free-firmware
deb http://deb.debian.org/debian-security bookworm-security main non-free-firmware


- Next press ctrl+x, then press y, and then press enter

- Install dependancies, SteamCMD, and update

```bash
sudo apt install software-properties-common
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install lib32gcc-s1 steamcmd
```
>SteamCMD should now be installed.

### **Setup SCDS**

- Make a directory for the server to go into

```bash
sudo mkdir /svends
sudo chmod 775 /svends
sudo chown yourusername /svends
```
>You can name the directory something else if you want to.

- Download and install SCDS with SteamCMD
```bash
steamcmd +login anonymous +force_install_dir "/svends" +app_update 276060 validate +exit
```
>It will need to download the server files for a bit, if you get an error at this point try the command again.

- Make sure everything installed to the correct directory
  
```bash
cd /svends
ls
```
>If you see files and more directories then it installed to the correct folder 

### Test and troubleshoot the server

- Test the server

```bash
./svends_run -console -port 27015 +maxplayers 4 +log on +map "hl_c01_a1"
```

If you are using a AMD CPU use this instead

```bash
./svends_run -console -binary ./svends_i686 -port 27015 +maxplayers 4 +log on +map "hl_c01_a1"
```
> If you don't get any errors and the server starts then this guide is starting to age and the next Sven Co-op update came out, move on to the next step. If you get a fatal error because of libssl.so.1.1 then make sure to follow the rest of this step.

- Install lib32z1

```bash
sudo apt install lib32z1
```
- Download libssl1.1 (DO NOT INSTALL THE DEB FILE)
https://packages.debian.org/bullseye/i386/libssl1.1/download

- With a archive manager, find and extract libcrypto.so.1.1 and libssl.so.1.1

- Place the files in /hlserver or whatever you named your server directory

>The issue with libssl.1.1 should now be fixed. If you are still getting errors you should get in contact with people on the [Sven Co-op Discord Server](https://discord.gg/svencoop). Ask for help in #server-operators.

### **Customizing the server**

*Get familiar with the setup*

```bash
cd /svends/svencoop
sudo nano banned.cfg
```

- Press enter, then ctrl+x, then y, then enter
>You just made a file called banned.cfg, place the steam ids of unwanted troublemakers in this file. You can seperate ids by pressing enter. The ids should look like STEAM_0:0:00000000

- Open server.cfg

```bash
sudo nano server.cfg
```

- Change the hostname

- Under log "on" add this text

```
sv_downloadurl "http://fastdl.boderman.net/"

sv_cheats 3
```
>This does two things, it added a fastdl server so your map downloads won't be so slow AND enabled cheats for the server owner. If you trust your admins you can use sv_cheats 2 instead, it allows admins to use cheats too.

- Add a password if you don't want random players using your server

- Press ctrl+x, then press y, and then press enter

- Open admins.txt

```bash
sudo nano admins.txt
```

- Add your steam id how the file says. If you already have someone you plan on adding as admin, add their steam id too

- Press ctrl+x, then press y, and then press enter 
>You should now be able to use cheats and kick commands. You and your admin can use these to better manage the servers and maybe give confuse players for laughs (pls no abuse) [SV BOY's Cheats Guide](https://www.youtube.com/watch?v=bof5Ji5S4Yg)

- Open motd.txt

```bash
sudo nano motd.txt
```
>What you put into this files is the message that people will see when they join your server. It's a good idea to put server info, group info, and contact information here.

- Customize the file until you are happy with it

- Press ctrl+x, then press y, and then press enter 
>Unless you have cutom maps you want to add already, the server is now setup.

### Custom Content 

This part requires you to find and download maps you want on your server at http://scmapdb.wikidot.com/

- Extract everything you downloaded from scmapdb then place it in all in /svends/svencoop_addon

- Now figure out which bsp files are the first maps to the content you want to host
>That part is important and we cannot help you on this step. Different maps have different names and a "map" usually consists of multiple maps. To clear up confusion you can call all the maps combined a map pack. What you download from scmapdb is a map pack unless it only has one bsp file in which case thats is just a map.

- Open mapcycle.txt

```bash
cd /svends/svencoop
sudo nano mapcycle.txt
``` 

- Add the bsp file names you got earlier but remove the .bsp file at the end.

- Press ctrl+x, then press y, and then press enter

- Open mapvote.txt

```bash
sudo nano mapvote.txt
```
- Repeat step 9 for this but at the start each map place "addvotemap"

>You should now be able to vote on the maps you added and the server will cycle through these maps automatically. 

### **Keep the server running** 

While you could get away with keeping the server running in a terminal window, it's prone to issues. Our prefered method of keeping servers running is GNU Screen. 

```bash
sudo apt install screen
```

If you don't want to use Screen, you can use [this guide](https://wiki.svencoop.com/Running_a_server/Advanced#Linux) to make a service instead. 

- Start screen
```bash
screen
```

If you have a screen already running then you should name this one so you can return to it easily.

```bash
screen -S svends
```

- Make sure you are in the correct directory

```bash
cd /svends
```

- Start the server 

```bash
./svends_run -console -port 27015 +maxplayers 8 +log on +map "hl_c01_a1"
```

If you are using a AMD CPU use this instead

```bash
./svends_run -console -binary ./svends_i686 -port 27015 +maxplayers 8 +log on +map "hl_c01_a1"
```
>Now when you close the terminal the server will stay up. To return to your screen use "screen -r svends"

### Admin Tasks

- If a player reports something that took place in chat logs then you can verify that this happened with the logs directory which can be found at /hlserver/svencoop/logs.

- You can kick players from the server with

```
kicksteamid "STEAM_0:0:00000000"
```
>Replace example id with player's steam id, keep the quotes.

- You can ban players by placing player's steam ids into the banned.cfg file we made earlier. For it to go into effect the server needs to change maps.

## Notes

- Keep the OS up to date

```bash
sudo apt update
sudo apt upgrade
```

- Keep the server up to date

```bash
steamcmd +login anonymous +force_install_dir "/svends" +app_update 276060 validate +exit
```

For the general purpose guide made by the SC Team please use [this guide](https://wiki.svencoop.com/Running_a_server), it has more important documentation that we do not mention on this guide

Message to the Sven Co-op Team: ``
Please explain what the server dependencies are and how to find them on your guides. Also please don't tell people to use libssl.1.1 without mentioning to not install the .deb file it comes in. Installing it like that breaks system updates``
