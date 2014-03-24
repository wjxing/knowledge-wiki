---
layout: post
title: "Compile and link 3rd libs"
date: 2014-03-24 13:14
category: "Linux"
tags: Compile Link
---
{% include JB/setup %}

在编译、链接的时候，有一些工具和环境变量会影响这个过程，总结一下：

------

## 涉及的工具

### 1. pkg-config
不同的平台对于相同的库可能存放到不同的路径下面，使用这个工具可以获取要使用库的编译信息，比如：

<pre>pkg-config --libs glib-2.0</pre>

可以得到使用python库时的链接信息，而

<pre>pkg-config --cflags glib-2.0</pre>

可以得到使用python库时的头文件路径信息，于是乎，在编译自己的程序时可以用如下的通用代码

<pre>gcc sample.c -o sample \`pkg-config --cflags --libs glib-2.0\`</pre>

这个工具是到特定的目录(比如/usr/lib/pkgconfig)下面去搜索.pc文件，而.pc文件中包含了特定库的原信息，比如头文件路径，链接库路径等
可以修改PKG\_CONFIG\_PATH来修改搜索的路径

### 2. ldconfig
这个工具会把/etc/ld.so.conf文件中指定的库搜索路径编译到/etc/ld.so.cache文件中，通过这个编译的文件加快搜索动态库的速度
如果在/etc/ld.so.conf或者/etc/ld.so.conf.d/里面添加或修改了路径，需要重新运行ldconfig来生成cache文件

## 涉及的环境变量或目录

### 1. LD\_LIBRARY\_PATH
是一组用逗号隔开的目录，在查找标准目录集之前先查找这些目录

### 2. LD\_PRELOAD
列出了覆盖标准集的函数所在的目标文件，就像/etc/ld.so.preload一样

### 3. /etc/ld.so.preload
如果你想覆盖某个库中的一些函数，用自己的函数替换它们，同时保留该库中其他的函数的话，
可以在/etc/ld.so.preload中加入你想要替换的库（.o结尾的文件），这些preloading的库函数将有优先加载的权利

------

于是在制作一个库需要给其他程序使用，并且不想使用configure等标准的工具也不想拷贝到系统目录下的时候可以这么做：

* 把头文件和库文件拷贝到特定的目录下，并且在/usr/lib/pkgconfig目录下创建.pc文件
* 在/etc/ld.so.conf.d/目录下增加.conf文件，以指定动态库的路径
