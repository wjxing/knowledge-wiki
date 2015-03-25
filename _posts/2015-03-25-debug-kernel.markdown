---
layout: post
title: "Debug Kernel"
date: 2015-03-25 12:39
category: Linux
tags: linux kernel debug
---
{% include JB/setup %}

基于虚拟机的Debug Kernel

------

### 1. 安装vbox虚拟机，然后在虚拟机中安装linux

### 2. 在linux虚拟机中更新软件源，并执行如下命令：
    sudo apt-get update
    sudo apt-get install linux-source
    sudo tar -xjf linux-source-$(VERSION).tar.bz2
    sudo apt-get install kernel-package libncurses5-dev build-essential

### 3. 配置minicom串口通信
在虚拟机设置中启用串口，一般端口选择使用COM1对应虚拟机的/dev/ttyS0，
端口模式选择Host Pipe，表示将虚拟机的串口连接到主机的一个命名管道，
创建通道表示启动虚拟机时VirtualBox会在HostOS中对应地创建一个命名管道，并使它与虚拟机的对应串口相连，
端口路径在选择Host Pipe时表示命名管道的路径，在Linux可以设任何路径，比如将它设为/tmp/vbox

修改~/.minirc.dfl文件，添加如下行
pu port unix#/tmp/vbox
然后启动minicom，在虚拟机终端中执行:
echo is that ok > /dev/ttyS0
会在minicom中回显对应内容
在虚拟机中的终端中执行：
cat /dev/ttyS0
这时host机中的minicom中会出现输入状态，在其中输入的信息也会在虚拟机终端上回显

### 4. 编译并安装内核以及模块
make all && make modules_install && make install && mkinitramfs -o /boot/initrd.img-$(VERSION)
update-grub

### 5. kgdb调试
将虚拟机中/usr/src/linux-source-$(VERSION)文件夹中的linux代码和vmlinux内核文件拷贝一份到host机对应目录下,
修改虚拟机中/boot/grub/grub.cfg文件，在编译好的内核的启动选项后加上
kgdboc=ttyS0,115200 kgdbwait
此时在host机上可以连接调试：
socat tcp-listen:8888 /tmp/vbox
/tmp/vbox是虚拟机串口的管道文件，socat将这个管道文件又重定向到宿主机的TCP 8888端口上
gdb ./vmlinux
set remotebaud 115200
target remote tcp:localhost:8888

上面这种方式是在启动阶段加断点，如果是启动以后可以如下方式启动断点：
在虚拟机端把串口设置为kgdb通道：
echo ttyS0 > /sys/module/kgdboc/parameters/kgdboc
虚拟机端进入debug状态：
echo g > /proc/sysrq-trigger
此时就可以通过上面的方式用gdb来调试
