---
layout: post
title: "The Learning Vimscript"
date: 2014-07-07 23:41
category: Vim
tags: [vim]
---
{% include JB/setup %}

学习Learn Vimscript the Hard Way记录一些笔记

------

### 1. Mappings

    nnoremap <buffer> <leader>x dd

只在当前buffer有效

### 2. Setttings
布尔类型的变量

    set number   "设置为true
    set nonumber "设置为false
    set number!  "设置为相反的值
    set number?  "查询当前值

有数值的变量

    set scrolloff=10

    setlocal wrap

设置只在当前buffer

### 3. Shadowing

    nnoremap <buffer> Q x
    nnoremap          Q dd

当以上面的顺序设置后，第一个会在当前buffer生效

### 4. Autocommand Event and FileType

    autocmd BufNewFile * :write
            ^          ^ ^
            |          | |
            |          | The command to run.
            |          |
            |          A "pattern" to filter the event.
            |
            The "event" to watch for.
    autocmd FileType javascript nnoremap <buffer> <localleader>c I//<esc>

### 5. Abbreviations

    iabbrev <buffer> --- &mdash;

当在当前buffer中输入`---`然后按空格后会被`&mdash;`代替

### 6. Autocommands and Abbreviations

    autocmd FileType python :iabbrev <buffer> iff if:<left>

当打开文件是python类型时，输入iff空格后，会用if:<left>代替

### 7. Grouping Autocommands

    autocmd BufWrite * :sleep 200m

如果多次载入上面的命令，则效果会叠加，并不会被覆盖，可以这样使用来避免

    augroup filetype_html
        autocmd! "清除之前这个group里面的autocmd定义
        autocmd FileType html nnoremap <buffer> <localleader>f Vatzf
    augroup END

### 8. Movement Mappings

    onoremap p i(

定义了以后，当输入dp时就如同输入了di(，删除括号里面的内容

### 9. Status Lines

    set statusline=%f\ -\ FileType:\ %y

设置后会在状态栏显示类似：foo.markdown - FileType: [markdown]的内容，
也可以写成多行

    set statusline=%f
    set statusline+=\ -\ 
    set statusline+=FileType:
    set statusline+=%y

另外，%=表示居右显示

### 10. General Format

    %-0{minwid}.{maxwid}{item}
    ^^^^        ^
    ||||        |
    ||||        item显示的最大宽度
    ||||
    |||item显示的最小宽度
    |||
    ||以0来填充空余item
    ||
    |左对齐显示item
    |
    特定前缀
