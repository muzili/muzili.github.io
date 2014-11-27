---
layout: post
category : lessons
tagline: "Supporting tagline"
tags : [intro, beginner, jekyll, tutorial]
---
{% include JB/setup %}

开始
====

我对IDE开发环境有着天然的抗拒，无论VS/Eclipse etc.
对于Android开发环境来说，到处充斥着使用Eclipse， 本系列博客
主要是探索如何使用单纯的开发环境来开发Android应用程序。

安装Android SDK/NDK
-------------------

-   Download and install SDK from[^1]
-   Download and install NDK from[^2]

创建AboveCamera应用
-------------------

### 列出当前sdk支持的target, 以备后用

> android list targets

### 使用android create创建地一个应用

> android create project --target 8 --name AboveCamera \\ --path .
> --activity MainActivity --package abovelink.wifi.qr

### 创建initial git repos

> git init git add . git ci -m "Initial create the AboveCamera with
> 'android create'"

### 编译AboveCamera

> ant debug

### 使用'android avd'创建AVD

> android avd

因为android系统的启动时间比较长，这是你可以去喝杯咖啡，
当然，我比较倾向于白开水。

### 在虚拟机中运行AboveCamera

> adn debug install

这时候就可以在虚拟机中看到你的第一个程序了。

Footnotes
=========

[^1]: <http://developer.android.com/sdk/index.html>

[^2]: <http://developer.android.com/tools/sdk/ndk/index.html>

    {{{more}}}

