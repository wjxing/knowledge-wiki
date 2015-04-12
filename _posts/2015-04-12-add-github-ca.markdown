---
layout: post
title: "Add GitHub CA"
date: 2015-04-12 10:44
category: Git
tags: git tools
---
{% include JB/setup %}

Add GitHub CA

------

如果使用git clone时出现了"server certificate verification failed"错误，可以使用如下命令获取github ca，然后添加到/etc/ssl/certs/ca-certificates.crt文件中即可

    echo -n | openssl s_client -showcerts -connect github.com:443 2>/dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p'
