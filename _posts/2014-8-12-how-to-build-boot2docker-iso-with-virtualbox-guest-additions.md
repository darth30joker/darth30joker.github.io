---
title: How to build a boot2docker ISO with VirtualBox Guest Additions
layout: post
tags: docker
---

In [previous post](/2014/06/10/have-fun-with-docker/), I talked about docker. But with boot2docker, there's a big issue, that is you cannot mount local directories in container.

To understand this, we need to know how boot2docker works. As we know, docker only runs on Linux kernels. So boot2docker uses VirtualBox to create a Linux VM and then install docker in it. Thus, every docker command we run on Mac/Windows, actually runs in this VM. This Linux is tinycore linux, you can search more information for tinycore linux.

The problem is, this Linux VM doesn't have VirtualBox Guest Additions(vga) installed. Without this, we cannot mount any local directories to VM. So our soluation is to install vga in tinycore linux.

Thanks to this [Pull Request](https://github.com/boot2docker/boot2docker/pull/284/files), we can build our own boot2docker.iso.

The basic idea of this is to pack vga drivers into tinycore linux before making an ISO.

Here's the steps of building ISO:

1. clone this repo: [boot2docker](https://github.com/boot2docker/boot2docker)
2. patch this [Pull Request](https://github.com/boot2docker/boot2docker/pull/284/)
3. `docker build -t boot2docker . && docker run --rm boot2docker > boot2docker.iso`
4. backup ~/.boot2docker/boot2docker.iso and copy your iso here
5. do the things listed here: [https://github.com/boot2docker/boot2docker/pull/284#issue-29404515](https://github.com/boot2docker/boot2docker/pull/284#issue-29404515)

There you go!

**NOTICE: If you want to know the details of building an ISO, please refer to this: [How to build boot2docker.iso locally](https://github.com/boot2docker/boot2docker/blob/master/doc/BUILD.md)**