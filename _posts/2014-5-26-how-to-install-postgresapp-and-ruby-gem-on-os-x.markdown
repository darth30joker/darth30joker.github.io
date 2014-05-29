---
layout: post
title: 如何在OS X里装postgresql和Ruby下的Gem
---

在OS X里装postgresql有很多方法:

* 使用EnterpriseDB. 这个是安装程序, 会把postgresql装在OS X里.
* 使用PostgresApp. 这个是绿色程序, 不需要安装, 打开后postgresql就能用了, 关掉程序, postgresql就停了. 非常适合偶尔使用的. (洁癖必选!)

本文主要讲如何使用PostgresApp来提供postgresql服务, 同时安装Ruby下的gem.

1. 下载PostgresApp: [查看各版本下载地址](https://github.com/PostgresApp/PostgresApp/releases)
2. 设置环境变量, `export PATH=/Applications/Postgres.app/Contents/Versions/9.3/bin:$PATH`
3. 使用`bundle install`或者其它命令来安装pg的gem
