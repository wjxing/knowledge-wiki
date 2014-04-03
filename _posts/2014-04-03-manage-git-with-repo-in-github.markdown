---
layout: post
title: "Manage git with repo in github"
date: 2014-04-03 12:50
category: "git"
tags: git
---
{% include JB/setup %}

Manage git with repo in github

------

repo工具的功能是对众多git仓库提供一个管理，相当于对git的封装，摒弃了git submodule的不便利，repo需要的是一个manifest文件，这个manifest包含了众多git仓库的信息，有趣的是这个manifest文件也是用一个git仓库来管理的。以上迹象表明，repo需要的就是git仓库，所以完全可以使用repo工具来管理github上面的仓库。

但是repo多了一个review的机制，需要配合gerrit服务器来使用，而github上是没有这种服务的！所以，本文包含的只是使用repo来下载代码，而具体的提交还是需要在各自的git仓库中进行。（可以修改repo相关的脚本，把review的机制去掉，但不在本文介绍之列）

### 创建manifest的git仓库
在github上创建新的仓库，然后在本地创建空仓库，并添加default.xml文件（使用repo init的时候如果不加-m参数会使用这个default.xml文件），文件内容大致如下

    <?xml version="1.0" encoding="UTF-8"?>
    <manifest>
        <remote name="origin"
            fetch=".." />

        <default revision="master"
            remote="origin"
            sync-j="4" />

         <project path="build" name="test_repo_git"/>
    </manifest>

这里的project标签就是需要管理的github仓库，具体的语法参考[Android Repo的manifest XML文件格式][1]。

之后就可以把代码上传到github上面。

### 使用repo下载代码
完成了上面的步骤之后就可以使用repo来下载代码了
<pre>
repo init -u $(上面步骤中使用的github仓库地址)
repo sync
</pre>

### 提交代码
上文也提到过，如果要上传代码的话，不能使用repo upload，因为没有gerrit服务器，所以还是需要在每个git仓库里面提交代码

[1]: http://blog.csdn.net/hansel/article/details/9798189
