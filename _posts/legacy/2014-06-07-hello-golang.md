---
layout: post
category : lessons
tagline: "Supporting tagline"
tags : [intro, beginner, jekyll, tutorial]
---
{% include JB/setup %}

Overview
========

从RSS上看到一则关于syncthing的消息，感觉不错，开源，
使用go语言，我之前适用aerofs, bittorrent sync, 都不是 太理想.
我自己也通过这个机会，学习一下golang的基础知识．

{{{more}}}

先checkout syncthing的代码
==========================

> cd workshop git clone <https://github.com/muzili/syncthing> cd
> syncthing git remote add upstream <https://github.com/calmh/syncthing>

Setup golang environment
========================

我使用debian sid, 所以可以直接适用apt-get安装golang

> sudo aptitude -y install golang golang-tools

Build syncthing
===============

=./build.sh setup=
------------------

这时遇到2个问题，一个是需要设置GOPATH,
一个是G\*F\*W封掉了code.google.com.

### 设置GOPATH

添加下面几行到 `.bashrc`

``` {.bash}
#setup gopath
#https://github.com/astaxie/build-web-application-with-golang/blob/master/ebook/01.2.md
GOPATH=$HOME/code/go
PATH=$PATH:$GOPATH/bin
```

### 正好前几天通过digitalocean建了一个OpenVPN, 正好用上

用下面的命令开始vpn

> \~/bin/ovpn

我在安装过程中遇到下面的错误，反正我的vet已经安装过了， 不管它．

``` {.example}
go install code.google.com/p/go.tools/cmd/vet: open /usr/lib/go/pkg/tool/linux_amd64/vet: permission denied
```

开始Build by `./build.sh`
-------------------------

这时出现了下面的错误, 看起来没有把当前目录加进去．

> cmd/syncthing/gui.<go:25:2>: cannot find package
> "github.com/calmh/syncthing/auto" in any of:
> /usr/lib/go/src/pkg/github.com/calmh/syncthing/auto (from \$GOROOT)
> /data/workshop/syncthing/Godeps/~workspace~/src/github.com/calmh/syncthing/auto
> (from \$GOPATH)
> /home/joshua/code/go/src/github.com/calmh/syncthing/auto ..

这时才发现go语言的build可能不是我想的样子，然后到syncthing 找到下面的
`build guide` <http://discourse.syncthing.net/t/building-syncthing/44>

> go get github.com/calmh/syncthing/cmd/syncthing

Run
---

> \$ syncthing [5XSMV] 09:43:40 INFO: syncthing unknown-dev (go1.2.1
> linux-amd64) unknown@unknown 1970-01-01 00:00:00 UTC [5XSMV] 09:43:40
> INFO: My ID: 5XSMVYOW7PGH77CK32VUAKST2ATQBDHC3SV4LDMS3CZL7GBUT5YQ
> [5XSMV] 09:43:40 INFO: Starting web GUI on <http://127.0.0.1:8080/>
> [5XSMV] 09:43:40 INFO: Populating repository index [5XSMV] 09:43:43
> INFO: No UPnP IGD device found, no port mapping created (read udp4
> 0.0.0.0:51036: i/o timeout) [5XSMV] 09:43:43 INFO: Sending local
> discovery announcements [5XSMV] 09:43:43 INFO: Sending global
> discovery announcements [5XSMV] 09:43:43 OK: Ready to synchronize
> default (read-write)

