---
layout: post
title: "GCC get real include file path"
date: 2015-10-27 17:15
category: gcc
tags: linux program gcc
---
{% include JB/setup %}

GCC提供了一些选项，可以获取c文件使用的头文件的完整路径

------

### -M 选项
    gcc -c -M file.c

以上命令会输出file.c文件使用的头文件的完整路径，但是不会编译file.c，等同于不加**-c**参数

    gcc -c -MD file.c

以上命令会编译file.c产生file.o文件，并且生成file.d文件，此文件包含file.c文件使用的头文件的完整路径

    gcc -c -MD -MF dep.d file.c

此命令可以指定生成的依赖文件的文件名
