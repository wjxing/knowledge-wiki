---
layout: post
title: "first post"
date: 2014-02-18 20:21
category: talk
tags: [introduce]
---
{% include JB/setup %}

这是第一篇post，留个印记！

------

## 记录一下搭建过程

* 这个jekyll模板来自[equation85][1]
* 由于我想基于`http://username.github.io/project`的basepath来搭建，所以修改了一下default.html中的路径，参考[提交1][2]和[提交2][3]
* 在本地使用rvm安装jekyll的时候，遇到了这个命令

    bash -s stable < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)

纠结了半天来理解这条命令：

解析：**bash -s stable < <(command)**

**<(command)** : 这里表示bash下的进程替换功能，把command的执行结果存到一个中间文件中（可以使用echo <(command)查看中间文件路径）

**bash -s** : 这里-s参数表示bash将把标准输入的内容当作命令执行，之后的内容是当成参数传递给传递来的命令

**<** : 这里表示把中间文件当作bash的标准输入

亦可以这么执行[@rvm][4]：

    curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer | bash -s stable

[1]:http://equation85.github.io
[2]:https://github.com/wjxing/knowledge-wiki/commit/a763abcc3c2d5fa2dfd266fe99191bfaa05a62c9
[3]:https://github.com/wjxing/knowledge-wiki/commit/74f24fcf42339a28f58892d738e9d7d0e48ec831
[4]:http://rvm.io/rvm/install
