---
layout: post
title: "Package Manager"
date: 2015-03-25 13:13
category: Linux
tags: linux tools
---
{% include JB/setup %}

Package Manager

------

### 1. dpkg
dpkg -i package.deb #安装deb
dpkg -c package.deb #列出deb内容
dpkg -I package.deb #提取包裹信息
dpkg -r #移除安装包
dpkg -P #完全清除安装包
dpkg -l package #参数是小写L，查找已经安装软件包 package 的信息，精简
dpkg -L package #列出安装的所有文件清单

echo "gaim hold" | dpkg --set-selections //设置gaim不能升级
dpkg --get-selections "gaim"

### 2. apt
apt-cache search #------(package 搜索包)
apt-cache show pattern #查找软件包pattern的信息 (可以是安装了也可以是没有安装)
apt-cache depends #-------(了解使用依赖)
apt-cache rdepends #------(查看该包被哪些包依赖)
apt-get source #------(package 下载该包的源代码)
sudo apt-get remove #-----(package 删除包)
sudo apt-get remove --purge #------(package 删除包，包括删除配置文件等)
sudo apt-get autoremove --purge #----(package 删除包及其依赖的软件包+配置文件等)
sudo apt-get update #------更新源
sudo apt-get upgrade #------更新已安装的包
sudo apt-get dist-upgrade #---------升级系统
sudo apt-get dselect-upgrade #------使用 dselect 升级
sudo apt-get build-dep #------(package 安装相关的编译环境)
sudo apt-get clean && sudo apt-get autoclean #--------清理下载文件的存档 && 只清理过时的包
sudo apt-get check #-------检查是否有损坏的依赖

### 3. aptitude
aptitude search ~i #查找已经安装的软件包
aptitude search ~c #查找已经被 remove 的软件包，还有配置文件存在
aptitude search ~npattern #查找软件包名称包含 pattern 的软件包 (可以是安装了也可以是没有安装)
aptitude search \!~i~npattern #查找还没有安装的软件包名字包含 pattern 的软件包。(前面的 ! 是取反的意思，反划线是 escape 符号)
aptitude search ~R~npackage #查找名称是 package 软件包的依赖关系，可以同时看到是不是已经安装
aptitude search ~D~npackage #查找哪些软件包依赖于名称是 package 软件包

aptitude show ~npattern #显示名称是 pattern 软件包的信息(可以是安装了也可以是没有安装)
