---
layout: post
title: CentOS git 编译
categories:
- Git
tags:
- Git
---
CentOS 6.2,git 1.7.9

官方yum库git版本较老

1.7.7 stash 才支持un tracked 的文件

官方的git-scm 被GFW屏蔽了,用xx门翻

下载地址

http://code.google.com/p/git-core/downloads/list

git-XX.tar.gz

参见源码中INSTALL文档

    $ make prefix=/usr all doc info ;# as yourself
    # make prefix=/usr install install-doc install-html install-info ;# as root

LINUX系统安装时不要选上git

安装几个依赖包，否则make 时报错

yum install curl-devel expat-devel gettext-devel  openssl-devel zlib-devel

完整安装还需要asciidoc,docbook2X

其中make info 时需要的docbook2X，不在centos iso中

`wget http://download.fedora.redhat.com/pub/epel/6/i386/epel-release-6-5.noarch.rpm`

`rpm -Uvh epel-release*rpm`

`yum install docbook2X`

安装好了docbook2x

`cd /usr/bin`

`ln -s db2x_docbook2texi docbook2x-texi`

不然make info时

`/bin/sh: line 1: docbook2x-texi: command not found`

无需设置环境变量,git 命令直接能用


参考

[为CentOS/RHEL添加EPEL软件仓库](http://fedoraproject.org/wiki/EPEL/zh-cn )

