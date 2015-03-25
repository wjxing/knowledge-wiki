---
layout: post
title: "CPP Build Message"
date: 2015-03-25 14:09
category: CPP
tags: cpp program
---
{% include JB/setup %}

CPP Build Message

------

### 1. 编译阶段打印宏内容
    #pragma message //它能够在编译信息输出窗口中输出相应的信息

* 用于测试的宏
    #define MAX(a,b) (a)>(b) ? (a) :(b)

* 两个辅助宏
    #define PRINT_MACRO_HELPER(x) #x
    #define PRINT_MACRO(x) #x"="PRINT_MACRO_HELPER(x)

* 编译阶段打印宏内容
    #pragma message(PRINT_MACRO(MAX(a,b)))

* 输出
    note: #pragma message: MAX(a,b)=(a)>(b) ? (a) :(b)
