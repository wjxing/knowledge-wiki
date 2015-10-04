---
layout: post
title: "Test kramdown"
date: 2015-10-04 10:29
category: talk
tags: introduce
---
{% include JB/setup %}

Test kramdown

------

### 测试转义字符
\\ \_ \*

### 测试换行
This one line.\\
This two line.

### 测试表格

|-----------------+--------------+----------------+---------------|
| Default aligned | Left aligned | Center aligned | Right aligned |
|-----------------|:-------------|:--------------:|--------------:|
|cell1            |cell2         |cell3           |cell4          |
|cell5            |cell6         |cell7           |cell8          |
|-----------------+--------------+----------------+---------------|

### 测试列表

1. Item one
   1. sub item one
   2. sub item two
2. Item two

* Item one
+ Item two
- Item three
