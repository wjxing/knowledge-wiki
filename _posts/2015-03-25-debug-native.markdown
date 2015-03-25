---
layout: post
title: "Debug Native"
date: 2015-03-25 12:52
category: Android
tags: android debug gdb tools
---
{% include JB/setup %}

Debug Native

------

调试Native时，host需要执行一些设置
### 1. 通过网络调试：
    target remote $IP:5039
    set solib-absolute-prefix out/target/product/$TARGET/symbols/
    set solib-search-path out/target/product/$TARGET/symbols/system/lib/
    shared

### 2. 通过adb线调试
    shell adb forward tcp:5039 tcp:5039
    set solib-absolute-prefix out/target/product/$TARGET/symbols/
    set solib-search-path out/target/product/$TARGET/symbols/system/lib/
    handle SIGPIPE nostop
    target extended-remote :5039

### 3. 可以把这些命令写到一个脚本文件中，执行gdb的时候通过-x参数指定脚本文件，并且可以定义函数
    define loadsymbols
        shell echo set solib-absolute-prefix $SOURCE_SYMBOLS_PATH >/tmp/tmp.symbolspath
        shell echo set solib-search-path $SOURCE_SYMBOLS_PATH >>/tmp/tmp.symbolspath
        shell echo target remote :$INPUT_GDBINIT_PORT >>/tmp/tmp.symbolspath
        source /tmp/tmp.symbolspath
        shell rm /tmp/tmp.symbolspath
    end

    loadsymbols
