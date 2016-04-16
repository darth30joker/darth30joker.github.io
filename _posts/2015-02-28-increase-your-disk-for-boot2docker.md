---
layout: post
title: Increase your disk size for boot2docker
tags: docker
---

You searched for this article, so I assume you know what boot2docker is and are using it. That's good, so am I.

But how many times have you got errors like `no space left` when you tried to pull a new image and running a command with container? I'll tell you what, there's a way to increase your disk size.

Find `~/.boot2docker/profile` or create one if you cannot find it. For Windows, try `%USERPROFILE%/.boot2docker/profile`. There's a configuration called `DiskSize`, change it to any size you want.

Make the whole configuraion file look like:

{% highlight bash %}
# boot2docker profile filename: /Users/lxie/.boot2docker/profile
Init = false
Verbose = false
Driver = "virtualbox"
SSH = "ssh"
SSHGen = "ssh-keygen"
SSHKey = "/Users/lxie/.ssh/id_boot2docker"
VM = "boot2docker-vm"
Dir = "/Users/lxie/.boot2docker"
ISOURL = "https://api.github.com/repos/boot2docker/boot2docker/releases"
ISO = "/Users/lxie/.boot2docker/boot2docker.iso"
DiskSize = 32000
Memory = 2048
SSHPort = 2022
DockerPort = 0
HostIP = "192.168.59.3"
DHCPIP = "192.168.59.99"
NetMask = [255, 255, 255, 0]
LowerIP = "192.168.59.103"
UpperIP = "192.168.59.254"
DHCPEnabled = true
Serial = false
SerialFile = "/Users/lxie/.boot2docker/boot2docker-vm.sock"
Waittime = 300
Retries = 75
{% endhighlight %}

Destroy your existing boot2docker-vm by running `boot2docker destroy`, and then use `boot2docker init` to create a new one.

Enjoy your new environment!
