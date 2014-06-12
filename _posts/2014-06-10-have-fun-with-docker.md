---
layout: post
title: 使用docker
---

## 什么是docker?

docker是一个Linux Container, 可以把OS运行在里面, 为用户提供一个隔离的, 虚拟的运行环境.

docker和virtualbox的区别就是, docker的container是构建在OS之上, 以进程的方式为用户提供服务, 而virtualbox提供的环境, 是一个完整的机器环境, 包括各种物理设备的虚拟, 像磁盘, 网络, USB等设备. 可以这么说, docker提供的container是一个比virtualbox更简单, 更便捷的容器, 在使用上, docker的速度也快于virtualbox.

## 安装docker

Docker刚刚release了它的1.0版本, 非常值得去体验啊.

安装docker其实非常简单, 根据自己的OS, 参照文档即可.

比如我是在Mac上装的docker, 使用的文档是: [Installing Docker on Mac OS X](http://docs.docker.com/installation/mac/)

## 几个概念

### container

可以理解为VM, 一个container就是一个VM.

### image

创建container的镜像, 一般是一个OS的镜像, 里面除了放OS的东西, 还可以放各种application需要的东西, 比如程序代码, 数据等.

## 使用docker

在docker里创建contrainer, 需要一个可用的image, [docker hub](https://hub.docker.com/)上有很多可用的image供大家使用, 比如说我是fedora的使用者, 我就可以用 `docker pull fedora:heisenbug` 来下载F20到本地. 下载成功后, 使用 `docker images` 来查看可用的images:

	lxie@SanFrancisco ~ $ docker images
	REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
	fedora-packager     heisenbug           8da72b37d6a4        9 hours ago         941.4 MB
	fedora              heisenbug           3f2fed40e4b0        5 days ago          372.7 MB

有了image后, 你可以创建自己的container来运行你的application或者使用你的virtual os了: `docker -t -i fedora:heisenbug /bin/bash`, 怎么样? 已经在你的OS里了吧?

## 结论

本文只是介绍了如何安装docker, 并且使用已经创建好的image来建立自己的container, 这一步其实只是进入docker世界里最基本的一步, 后面我会继续介绍docker的命令和使用技巧.