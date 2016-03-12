---
layout: post
title: "Create commit from plumbing for git"
date: 2015-10-27 17:15
category: git
tags: git
---
{% include JB/setup %}

使用git底层命令来创建提交，装个X吧

------

### 为修改后的文件生成object
    git hash-object -w *file*

### 更新index
    git update-index *file*

此条命令执行后修改的文件会进入staged状态

### 创建tree object
    new_master_tree=$(git write-tree)

### 创建commit object
    commit=$(git commit-tree *TREE_SHA1* -p master -m "Create from plumbing")

### 更新master引用
    git update-ref refs/heads/master $commit

### 参考
[使用 git 底层命令创建提交](http://lilydjwg.is-programmer.com/2015/2/8/create-git-commits-with-plumbing-commands.79611.html)
