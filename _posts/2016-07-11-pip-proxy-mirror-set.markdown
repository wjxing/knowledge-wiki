---
layout: post
title: "pip Proxy/Mirror Set"
date: 2016-07-11 17:15
category: python
tags: python
---
{% include JB/setup %}

给pip设置代理或者设置mirror的方法

------

### 安装单个package的时候使用proxy，还可以加上默认超时
    pip install *package* --proxy=*proxy addr* --default-timeout=100

### 安装单个package的时候使用mirror
    pip install *package* -i https://*mirror*/simple

### 设置全局配置文件~/.pip/pip.conf
    [global]
    timeout = 6000
    index-url = http://*mirror*/simple/
    [install]
    use-mirrors = true
    mirrors = http://*mirror*/simple/
    trusted-host = *mirror*

### 参考
[pypi-mirrors](https://pypi-mirrors.org/)
