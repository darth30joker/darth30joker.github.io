---
layout: post
title: Use CLI to configure Wifi on debian for Raspberry Pi
tags: linux,debian,raspbian
---

Raspbian is a customized Debian on Raspberry Pi, and it's almost same as Debian.

Thank god Raspberry Pi 3 builds with a wireless module, we don't need to spend $10 on a USB wifi card.

This post talks about how to configure wifi on CLI mode.

1. Update `/etc/network/interfaces` and make sure it looks like below:

    ```
    auto lo
    iface lo inet loopback

    allow-hotplug wlan0
    auto wlan0

    iface wlan0 inet manual
    wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf
    ```

2. Create or update `/etc/wpa_supplicant/wpa_supplicant.conf`

    ```
    update_config=1
    network={
      ssid="Your Wifi Network"
      #psk="your password"
      psk=<encryped_passphase>
    }
    ```

    **Attention: wpa will never use plain password for connecting wifi, you need to passphase it and put it here.**

    Actually there's a simple way to generate this file, just use command below:

    ```
    wpa_passphrase <your_wifi_name> <your_wifi_password> > /etc/wpa_supplicant/wpa_supplicant.conf
    ```

    After this, just put `update_config=1` in the first line of that file.

3. Restart OS and you will see your Raspberry Pi is already connected to Wifi
