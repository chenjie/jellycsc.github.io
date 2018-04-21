---
layout: post
title:  "Essential Linux Binaries"
date:   2018-04-18 15:17:54
categories: Linux
tags: Tutorial
---

* content
{:toc}

我自从大一开始接触Linux至今已经有三个年头了，对Linux里面的命令或多或少都有些了解，但是每当我想用Terminal真正着手做些什么时，总需要回去翻翻StackOverflow. 归根结底是我对常用命令还不够熟悉，所以我就想在这篇文章里对常用Linux命令做一个总结，这既可以算作是给新手的入门介绍，也可以作为自己以后的参考。





## 简介
说到Linux，想必对于点开本文的你来说应该早有耳闻，在此我就不多做介绍了。这篇文章中所介绍的都是常用Linux二进制程序，也就是已经编译好的程序文件，像`cd`,`ls`等等，他们保存在/bin以及/usr/bin文件夹内。

## 目录
若你正在使用手机浏览本文，你可以通过点击屏幕右上方的`船锚`按钮来快速定位文章内容。  
若你用的是电脑，屏幕右侧边栏的导航正是为你准备的。

## `pwd`
我们先从最简单的命令入手，`pwd`显示的是当前你所在的路径。（注：以`/`开头的都是绝对路径）
```bash
$ pwd
/Users/jackni
```

## `cd`
切换当前路径，这个就等同于Windows里双击进入文件夹，或者按返回回到上一个目录。你可以输入相对路径或者绝对路径，前者是相对于你当前所在的路径（也就是`pwd`），后者跟当前路径无关，代表一个文件/文件夹的完整（绝对）路径。（注：结尾的`/`指明这是文件夹的路径）
```bash
# 相对路径
$ cd Desktop/folder/
$ pwd
/Users/jackni/Desktop/folder
```
```bash
# 绝对路径
$ cd /Users/jackni/Desktop/folder/
$ pwd
/Users/jackni/Desktop/folder
```

## `ls`
列出当前路径下的内容，这个就等同于Windows里，你在文件管理器中看到的当前文件夹里有什么东西。
```bash
# 默认输出
$ ls
blog_posts demo.py    movie      music
```
```bash
# -a显示隐藏文件
$ ls -a
.          ..         blog_posts demo.py    movie      music
```
```bash
# 加-l以列表形式显示（注：-al为组合flag，等同于ls -a -l）
$ ls -al
total 0
drwxr-xr-x   6 jackni  staff   192 20 Apr 16:24 .
drwxr-xr-x+ 35 jackni  staff  1120 18 Apr 16:24 ..
drwxr-xr-x   4 jackni  staff   128 18 Apr 16:38 blog_posts
-rw-r--r--   1 jackni  staff     0 20 Apr 16:24 demo.py
drwxr-xr-x   2 jackni  staff    64 18 Apr 16:26 movie
drwxr-xr-x   2 jackni  staff    64 18 Apr 16:26 music
```
```bash
# -F在文件夹结尾加/以便区分
$ ls -aF
./          ../         blog_posts/ demo.py     movie/      music/
```

## `top`
Linux里的任务管理器。
```bash
$ top
# 在界面中按o(order)打上cpu回车可以按照cpu占用来对所有进程排序
```

## `htop`
界面更人性化的`top`。
```bash
$ htop
# 按h(help)查看使用指南
```

## `man`
用户使用手册。（**非常重要**）不会使用这条命令相当于你学文言文不会查字典= =写到这条莫名的想起了初高中学语文的痛苦经历。
```
MANUAL SECTIONS
    The standard sections of the manual include:

    1      User Commands
    2      System Calls
    3      C Library Functions
    4      Devices and Special Files
    5      File Formats and Conventions
    6      Games et. al.
    7      Miscellanea
    8      System Administration tools and Daemons

    Distributions customize the manual section to their specifics,
    which often include additional sections.
```
```bash
# C语言标准函数库中的perror函数
$ man 3 perror
```
```bash
# chmod命令/程序
$ man 1 chmod
# 或1可省略
$ man chmod
```

## `tar`
Linux的tape存档工具，我算了解的功能和Winrar压缩解压类似。
```bash
# 打包此文件到pack.tar
$ tar -cvf pack.tar demo.py
a demo.py
$ ls
blog_posts demo.py    movie      music      pack.tar
```
```bash
# 解压pack.tar
$ tar -xvf pack.tar
x demo.py
```

## `grep`
文本式样搜索工具。

<img src="/mdres/loading.gif" width="20"/>持续更新中。。。
