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

### 11. Variables

    let foo = "bar"
    echo foo
    let foo = 42
    echo foo

变量都是动态类型变量

    let b:foo = "bar"

当前buffer有效的变量

### 12. Options as Variables

    set textwidth=80
    echo &textwidth

如上方式引用option的值，对于boolen型的option，1代表true，0代表false

    let &textwidth = &textwidth + 10

可以使用let对option赋值

    let &l:number = 1

设置local option

### 13. Registers as Variables

    let @a = "hello"

设置寄存器**a**的内容是hello，用y复制的内容保存在**"**寄存器里面，搜索的pattern保存在**/**寄存器里面

### 14. Multiple-Line Statements

    echom "foo" | echom "bar"

可以把多条命令写在一行并用**|**隔开

### 15. Code Defensively
基本的**==**判断结果依赖于用户设置的大小写是否敏感，比如用户设置

    set ignorecase

当用if "foo" == "FOO"比较时，结果是为true的，但是用户设置

    set noignorecase

结果是为false的，所以最好的选择是：使用**==?**作为大小写不敏感的判断，而使用**==#**作为大小写敏感的判断，它们都不受用户设置的影响

### 16. Calling Functions
可以使用call命令调用函数，当函数有返回值时，也可以使用echom等获取函数返回值

### 17. Function Arguments

    function DisplayName(name)
      echom "Hello!  My name is:"
      echom a:name
    endfunction

函数内引用参数时需要**a**前缀

    function Varg2(foo, ...)
      echom a:foo
      echom a:0
      echom a:1
      echo a:000
    endfunction

使用不定参数时，可以用**a:0**获取不定参数的个数（不包含确定参数），**a:1**获取第一个不定参数等，**a:000**以列表形式获取所有不定参数

### 18. Number Formats

    echom 100  "100
    echom 0xff "255
    echom 010  "8
    echom 019  "19

可以使用8、10、16进制数，对于8进制如果数字不符合8进制，则还是获得10进制值

### 19. Concatenation

    echom "Hello, " + "world"  "0
    echom "3 mice" + "2 cats"  "5
    echom 10 + "10.10"         "20
    echom "Hello, " . "world"  "Hello, world
    echom 10 . "foo"           "10foo
    echom 10.1 . "foo"         "Error

数字相加使用**+**，对于字符串会先转换成数字；字符串连接使用**.**；vim乐于让字符串转换成浮点数，但反过来会报错

### 20. Special Characters

    echom "foo\\bar"

会显示**foo\\bar**

    echo "foo\nbar"

会显示两行

    echom "foo\nbar"

会显示**foo^@bar**，echom输出字符串中额外的字符

### 21. Literal Strings

    echom '\n\\'

使用单引号来防止转义，输出字符串字面值

### 22. Splitting

    echo split("one two three")
    echo split("one,two,three", ",")

**split**把字符串转换成**list**

### 23. Joining

    echo join(["foo", "bar"], "...")

**join**把**list**转换成字符串

### 24. Basic Execution

    execute "rightbelow vsplit " . bufname("#")

把字符串解析成命令执行，**bufname("#")**获得上一个buffer的名称

### 25. Normal

    normal ggdd

在normal模式下执行ggdd命令，会扩展用户自定义的命令，使用normal!来执行原始默认的命令。normal!不认识特殊的字符串，比如**`<cr>`**

### 26. Execute Normal

    execute "normal! mqA;\<esc>`q"

execute可以编程式的构建执行的命令，并且弥补了normal!不能执行特殊字符串的缺点，而normal!会在normal模式下执行，并且忽略自定义的命令。

    execute "normal! gg/for .\\+ in .\\+:\<cr>"

如果normal!命令中需要转移字符**\\**，那么在构建execute字符串时需要**\\\\**

    execute "normal! gg" . '/for .\+ in .\+:' . "\<cr>"

另外可以使用字面值来表示正则表达式部分。对于正则表达式，可以使用**\v**(very magic)来避免使用转移字符，比如：

    execute "normal! gg" . '/\vfor .+ in .+:' . "\<cr>"

### 27. Case Study

    execute "grep! -R " . shellescape(expand("<cWORD>")) . " ."

当前目录下搜索光标所在的单词，其中`expand("<cWORD>")`会展开当前光标所在的单词，shellescape把这个单词用时候shell理解的转移符合包裹起来

### 28. Map-Operator

    nnoremap <leader>g :set operatorfunc=GrepOperator<cr>g@

这里定义了一个operatorfunc，当g@后面跟着**motion** command的时候，operatorfunc指定的函数会被执行，并且会传递一个type参数：

    
    "line"  {motion} was |linewise|
    "char"  {motion} was |characterwise|
    "block" {motion} was |blockwise-visual|

**\`[** mark指示**motion**开始的位置，**\`]**指示**motion**结束的位置

### 29. Function Namespacing
在自己的插件中编写函数时，最好使用**function! s:GrepOperator**这样的形式来定义本地化的函数，而不是定义一个全局的函数，那么在使用的时候可以这样调用：

    call <SID>GrepOperator()
