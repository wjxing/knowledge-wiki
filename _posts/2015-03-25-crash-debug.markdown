---
layout: post
title: "Crash Debug"
date: 2015-03-25 9:20
category: Android
tags: android debug
---
{% include JB/setup %}

Crash Debug

------

    setprop debug.db.uid pid_lower

设置了这个prop之后，小于pid_lower的进程crash的时候，可以使用gdbserver attach，来debug这个进程
