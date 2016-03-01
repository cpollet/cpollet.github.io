---
layout: post
title: Docker host on Raspberry Pi (installation from OS X)
---

# Download image
Download linux image from http://blog.hypriot.com/downloads/

# Install it on SD card

```
localhost$ diskutil list
localhost$ diskutil unmountdisk /dev/diskX
localhost$ cat hypriot-rpi-yyymmdd-hhmmss.img | pv -s `stat -f %z hypriot-rpi-yyyymmdd-hhmmss.img` | sudo dd bs=1m of=/dev/diskX
```
You may beed to install the `pv` tool with something like `bower install pv`)

# Enable internet sharing on your Mac

1. Open System Preferences
1. Select Sharing Preference Pane
1. Enable Internet Sharing with the following settings: Share your connection from: Wi-Fi to computers using Ethernet

# Boot the Raspberry and first login

Install the card, connect the ethernet cable and power up the PI. Then, discover its IP address with (if `nmap` is missing, you can install it via `bower install nmap`)

```
localhost$ ifconfig
localhost$ nmap -sP 192.168.2.1/24
```

where `192.168.1.2`  is the IP address of your bridgeXXX interface, given by `ifconfig`.

You can now login to your PI (considering its IP address is `192.168.2.2`):

```
localhost$ ssh root@192.168.2.2
password: hypriot
```

Alternatively, you can copy your public SSH key with

```
localhost$ cat ~/.ssh/id_rsa.pub | ssh root@192.168.2.2 "mkdir ~/.ssh; cat >> ~/.ssh/authorized_keys"
password: hypriot
localhost$ ssh root@192.168.2.2
```

# Play with docker

You should now be all set! To confirm that everything went fine, you can type:

```
raspberry$ docker info
```

Sources:

- [Hypriot.com blog](http://blog.hypriot.com/getting-started-with-docker-and-mac-on-the-raspberry-pi/)