---
layout: post
title: "GCC builtin Atomic Memory Access"
date: 2015-09-21 19:18
category: gcc
tags: gcc linux program
---
{% include JB/setup %}

GCC builtin Atomic Memory Access

------

GCC内置了一些原子操作的函数用于操作1, 2, 4 or 8 字节长度的指针类型或者integral scalar（[Legacy __sync Built-in Functions for Atomic Memory Access][1]）

type __sync_fetch_and_add (type *ptr, type value, ...)

type __sync_fetch_and_sub (type *ptr, type value, ...)

type __sync_fetch_and_or (type *ptr, type value, ...)

type __sync_fetch_and_and (type *ptr, type value, ...)

type __sync_fetch_and_xor (type *ptr, type value, ...)

type __sync_fetch_and_nand (type *ptr, type value, ...)

这些函数执行如它们名字所描述的操作，然后返回操作之前的值：

    { tmp = *ptr; *ptr op= value; return tmp; }
    { tmp = *ptr; *ptr = ~(tmp & value); return tmp; }   // nand

type __sync_add_and_fetch (type *ptr, type value, ...)

type __sync_sub_and_fetch (type *ptr, type value, ...)

type __sync_or_and_fetch (type *ptr, type value, ...)

type __sync_and_and_fetch (type *ptr, type value, ...)

type __sync_xor_and_fetch (type *ptr, type value, ...)

type __sync_nand_and_fetch (type *ptr, type value, ...)

这些函数执行如它们名字所描述的操作，然后返回操作之后的值：

    { *ptr op= value; return *ptr; }
    { *ptr = ~(*ptr & value); return *ptr; }   // nand

bool __sync_bool_compare_and_swap (type *ptr, type oldval, type newval, ...)

type __sync_val_compare_and_swap (type *ptr, type oldval, type newval, ...)

这些函数执行原子的比较和交换的操作，如果*ptr的值等于oldval，把*ptr赋值为newval

[1]:https://gcc.gnu.org/onlinedocs/gcc-5.2.0/gcc/_005f_005fsync-Builtins.html
