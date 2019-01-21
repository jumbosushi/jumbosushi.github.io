---
layout: post
title: "How to create shared folder on Windows Remote Desktop from Ubuntu via Remmina"
categories: blog
published: true
permalink: /:categories/how-to-create-shared-folder-on-windows-vm-from-ubuntu-with-Remmina/
---

As part of [COMM335](https://mybcom.sauder.ubc.ca/courses-money-enrolment/courses/comm-335) at UBC, students use Microsoft Access via Remote Desktop if you use Mac. Unfortunately, they don't have a guide on Ubuntu (yet). I had some trouble creating a shared folder between faculty's Windows VM and my Ubuntu 16.04.

Specifically my local computer wasn't accessible from Access unlike how it was showed in the Mac guide.

![mac_guide_pic](https://user-images.githubusercontent.com/9669739/51080253-d79af500-168c-11e9-82f4-f505a777613b.png)

(Picture taken from faculty's guide for Mac)


This short guide shows how to do it in case future students run into the same issue ⚡️

### Set up

I use [Remmina](https://remmina.org/) which is the remote desktop application that comes with Ubuntu. Default version installed on Ubuntu isn't up to date so recommended to get the latest version. This guide was done in Remmina version 1.2.32.1.

### Step 1

Follow the faculty's setup guide for Mac to get the `.rdp` file.

### Step 2

Open Remmina's Main Window, and find `import` option from its top right hamburgur menu.
Import the previously saved `.rdp` file.

![rem1](https://user-images.githubusercontent.com/9669739/51066471-27ab8600-15bf-11e9-9684-d20072675aa9.png)

Once imported, you should see sauder connection row in the Main Window

![rem2](https://user-images.githubusercontent.com/9669739/51066602-11ea9080-15c0-11e9-9496-e3a8aae90bad.png)

### Step 3

Remmina creates a configuration file for each setting it imported.
Depending on your setup, this config file should be under `~/.remmina` or `~/.config/remmina` with the name something like `1547174819500.remmina`
Find this file and open it in a text editor. It looks something like this on my set up.

```
[remmina]
name=SAUD-SRDS3P.EAD.UBC.CA
proxy=
ssh_enabled=0
exec=||MSACCESS
showcursor=0
colordepth=32
server=SAUD-SRDS3P.EAD.UBC.CA
loadbalanceinfo=tsv://MS Terminal Services Plugin.1.QuickSessionCollection
ssh_auth=0
aspectscale=0
ssh_charset=
quality=0
disableencryption=0
ssh_loopback=0
group=
ssh_username=
username=
hscale=0
gateway_server=saud-srds3p.ead.ubc.ca
window_maximize=0
gateway_usage=1
viewmode=1
viewonly=0
window_height=841
keymap=
window_width=1024
ssh_server=
shareprinter=1
protocol=RDP
disableserverinput=0
disableclipboard=0
ssh_privatekey=
vscale=0
last_success=20190111
```

**Remmina enables shared folder if `sharedfolder` field exists in the configuration!** Add the following line to the config file (I have it set up to share one of my Dropbox folder):

```
[remmina]
...
sharefolder=/home/atsushi/Dropbox/UBC/2019w/COMM335
...
```

That's it! :rocket: You should be able to access the specified folder in your Windows VM (for COMM335 via Access).

Remmina is open source and they have an [issue page on their GitLab](https://gitlab.com/Remmina/Remmina/issues), so you might be able to get help from there if you get stuck.

Hope this helps!
