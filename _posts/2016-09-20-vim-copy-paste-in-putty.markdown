---
layout: post
title: "Vim copy paste in putty"
date: 2016-09-20 11:27
category: Vim
tags: vim linux tools
---
{% include JB/setup %}

在putty中使用vim的复制和粘贴

------

### 在使用putty的windows端安装Xming，地址在
    https://sourceforge.net/projects/xming/

### 安装后，可以创建快捷方式，并在属性Target后面添加-ac参数，来禁止权限限制，并且确认linux端的/etc/ssh/sshd_config中，X11Forwarding为yes

### 在putty配置的Connection->SSH->X11中勾选Enable X11 forwarding，并在X display location后填写127.0.0.1:0

### putty连接后，执行
    export DISPLAY=${windows ip address}:0

### 之后使用tmux等就可以在vim中正常使用"+y来复制，然后"+p来粘贴了
