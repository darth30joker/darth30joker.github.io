---
layout: post
title: Install Raspbian image on Raspberry Pi 3
tags: linux,raspbian,raspberrypi
---

For SBC, a.k.a Single Board Computer, `dd` is our friend. Normally, this is the only tool we use to "install" OS.

Today I will take "Raspbian" for an instance. As you can see, Raspbian is debian for Raspberry Pi, and its latest version May 2016, released in 2016-05-27. Download link: [https://www.raspberrypi.org/downloads/raspbian/](https://www.raspberrypi.org/downloads/raspbian/)

After unzipping it, we get a file named `2016-05-27-raspbian-jessie-lite.img`.

OK, let's get started.

1. Prepare MicroSD

    My work environment is Mac OS X, so I just open `Disk Utility`, click on "Eject" button of my disk. Also, check device name on the right , which is some name like `disk2` or `disk3`. Mine here is `disk3`.

2. Run `dd`

    Open terminal, run below command:

    ```
    sudo dd bs=1m if=/path/to/your/2016-05-27-raspbian-jessie-lite.img of=/dev/rdisk<N>
    ```

    Here, we must pay attention to `/dev/rdisk<N>`. If you get `disk2` above, use `rdisk2` here, and if you get `disk3`, use `rdisk3`.

3. Finish

    After a while, `dd` puts the whole image into our MicroSD card, bad thing is there's no output of `dd`, so we never know when it is done. Once it's finished, all we need to do is to eject our card, and put it into Raspberry Pi and power on.
