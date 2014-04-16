---
layout: post
title: "Install grub in udisk"
date: 2014-04-16 11:03
category: Linux
tags: [linux grub]
---
{% include JB/setup %}

可以把grub安装到u盘上，并且把iso拷贝上去，从而使用u盘上grub引导iso去按照系统

------

## 把grub安装到u盘，终端下使用如下命令：

    # grub-install --boot-directory=/media/udisk /dev/sdb

上面命令中的`udisk`和`sdb`要根据具体环境改变

## 创建iso目录

在/media/udisk/grub目录下创建iso目录，并把linux的相关iso文件拷贝进去

## 修改grub.cfg文件

修改/media/udisk/grub/grub.cfg文件，添加类似如下的内容：

    menuentry 'ubuntu-12.04-desktop-amd64.iso' {
        insmod fat
        insmod loopback
        insmod iso9660
        loopback loop (hd0,1)/grub/iso/ubuntu-12.04-desktop-amd64.iso
        set root=(loop)
        linux /casper/vmlinuz.efi boot=casper iso-scan/filename=/grub/iso/ubuntu-12.04-desktop-amd64.iso
        initrd /casper/initrd.lz
    }

可以添加多个menuentry

## 最后就可以使用u盘启动去安装ubuntu
