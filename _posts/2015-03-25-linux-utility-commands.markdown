---
layout: post
title: "Linux Utility Commands"
date: 2015-03-25 9:03
category: Linux
tags: linux tools
---
{% include JB/setup %}

Linux Utility Commands

------

### 1. grub rescue修复
原因：grub2分为两部分，一部分（一般情况下）写在了mbr上，另一部分写在了某个分区的/boot/grub目录
（如果/boot单独分区，则直接写在对应分区的/grub目录）里面。
由于一些操作，致使grub2的mbr里面的那一部分找不到/grub目录里面的那一部分了（或者那一部分已经删除了）。
方法：
A. 先使用ls命令，罗列所有的磁盘分区信息
B. 使用ls (hd0,X)/boot/grub （/boot没有单独分区）或者ls （hd0,X)/grub （/boot单独分区）
C. 找到正确的grub目录
以下是/boot没有单独分区的命令：
    grub rescue>set root=(hd0,5)
    grub rescue>set prefix=(hd0,5)/boot/grub
    grub rescue>insmod /boot/grub/normal.mod

以下是/boot 单独分区的命令：
    grub rescue>set root=(hd0,5)
    grub rescue>set prefix=(hd0,5)/grub
    grub rescue>insmod /grub/normal.mod

然后调用如下命令，就可以显示出丢失的grub菜单了。
    grub rescue>normal

启动起来，进入ubuntu之后，在终端执行：
    sudo update-grub
    sudo grub-install /dev/sda
（sda是硬盘号码，千万不要指定分区号码，例如sda1，sda5等都不对）

### 2. screen同步
User A# screen -S debug
User B# screen -x debug

### 3. 输入并保存到文件
    cat > temp << EOF
    ...
    EOF

### 4. linux下找出最深的文件目录路径
find /home -type d -printf "%d %p\n" | sort -nrk1 | awk 'NR==1{a=$1}a==$1{print $2}'

### 5. ssh 自动登录
    ssh-keygen -t rsa -b 2048
    scp ~/.ssh/id_rsa.pub user@ssh-server:~/.ssh/authorized_keys

修改~/.ssh/config，格式如下：
Host server 名称自定义
    HostName 服务器地址
    User 登录用户名
    IdentityFile 证书文件路径
    Port 端口

登录的时候使用如下命令：
    ssh server

### 6. 进程替换
实例：
    bash -s stable < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)
这条命令是下载rvm-installer脚本，之后执行并且传递stable参数
解析：
    bash -s stable < <(command)
<(command) : 这里表示bash下的进程替换功能，把command的执行结果存到一个中间文件中（可以使用echo <(command)查看中间文件路径）
bash -s : 这里-s参数表示bash将把标准输入的内容当作命令执行，之后的内容是当成参数传递给传递来的命令
< : 这里表示把中间文件当作bash的标准输入

### 7. find过滤路径
find . -path "./out" -prune -o -name "XXX" -print
搜索XXX文件，但不包含./out目录下的

### 8. blkid - View UUID

### 9. alsamixer - Speaker Test

### 10. aplay -l - sound card

### 11. w - List Login Users

### 12. lsof（list open files）是一个列出当前系统打开文件的工具

lsof输出各列信息的意义如下：
COMMAND：进程的名称
PID：进程标识符
USER：进程所有者
FD：文件描述符，应用程序通过文件描述符识别该文件。如cwd、txt等
TYPE：文件类型，如DIR、REG等
DEVICE：指定磁盘的名称
SIZE：文件的大小
NODE：索引节点（文件在磁盘上的标识）
NAME：打开文件的确切名称

其中FD 列中的文件描述符
cwd 值表示应用程序的当前工作目录，这是该应用程序启动的目录，除非它本身对这个目录进行更改。
txt 类型的文件是程序代码，如应用程序二进制文件本身或共享库。其次数值表示应用程序的文件描述符，这是打开该文件时返回的一个整数。
u 表示该文件被打开并处于读取/写入模式，而不是只读 ® 或只写 (w) 模式。同时还有大写 的W 表示该应用程序具有对整个文件的写
锁。该文件描述符用于确保每次只能打开一个应用程序实例。
初始打开每个应用程序时，都具有三个文件描述符，从 0 到 2，
分别表示标准输入、输出和错误流。
与 FD 列相比，Type 列则比较直观。文件和目录分别称为 REG 和 DIR。而CHR 和 BLK，分别表示字符和块设备；
或者 UNIX、FIFO 和 IPv4，分别表示 UNIX 域套接字、先进先出 (FIFO) 队列和网际协议 (IP) 套接字。

lsof语法格式是：
lsof ［options］ filename
常用的参数列表：
lsof  filename 显示打开指定文件的所有进程
lsof -a 表示两个参数都必须满足时才显示结果
lsof -c string   显示COMMAND列中包含指定字符的进程所有打开的文件
lsof -u username  显示所属user进程打开的文件
lsof -g gid 显示归属gid的进程情况
lsof +d /DIR/ 显示目录下被进程打开的文件
lsof +D /DIR/ 同上，但是会搜索目录下的所有目录，时间相对较长
lsof -d FD 显示指定文件描述符的进程
lsof -n 不将IP转换为hostname，缺省是不加上-n参数
lsof -i 用以显示符合条件的进程情况
lsof -i[46] [protocol][@hostname|hostaddr][:service|port]
    46 --> IPv4 or IPv6
    protocol --> TCP or UDP
    hostname --> Internet host name
    hostaddr --> IPv4地址
    service --> /etc/service中的 service name (可以不只一个)
    port --> 端口号 (可以不只一个)

### 13. Set Resolution
    cvt 1440 900

显示：
1440x900 59.89 Hz (CVT 1.30MA) hsync: 55.93 kHz; pclk: 106.50 MHz
Modeline "1440x900_60.00" 106.50 1440 1528 1672 1904 900 903 909 934 -hsync +vsync

    xrandr --newmode "1440x900_60.00" 106.50 1440 1528 1672 1904 900 903 909 934 -hsync +vsync
    xrandr --addmode VGA1 "1440x900_60.00"
    xrandr --output VGA1 --mode "1440x900_60.00"
