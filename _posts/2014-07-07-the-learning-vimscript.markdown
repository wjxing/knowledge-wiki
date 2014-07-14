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

### 30. List

    echo ['foo', [3, 'bar']]

**List**可以嵌套

    echo [0, [1, 2]][-2]

**List**可以索引，-1表示最后一个元素，依此类推

    echo [0, 1, 2, 3][1:]

**List**可以取范围，并且是可以单向范围

    echo "abcd"[-1]  "空
    echo "abcd"[-1:] "d

字符串也可以取索引，只不过如果单单是一个数字索引，返回的是空

    echo ['a', 'b'] + ['c']

**List**可以连接

### 31. List Functions

    let foo = ['a']
    add(foo, 'b')
    echo foo        "['a', 'b']

**add**函数

    echo get(foo, 0, 'default')   "a
    echo get(foo, 100, 'default') "default

**get**函数

    echo index(foo, 'b')    "1
    echo index(foo, 'nope') "-1

**index**函数

    echo join(foo)           "a b
    echo join(foo, '---')    "a---b
    echo join([1, 2, 3], '') "123

**join**函数

    call reverse(foo)
    echo foo          "['b', 'a']

**reverse**函数

### 32. For Loops

    for i in list
    endfor

**for**循环能够变量**list**或者**dictionary**

### 33. While Loops

    while c <= 4
    endwhile

**while**循环

### 34. Dictionaries

    echo {'a': 1, 100: 'foo',} "{'a': 1, '100': 'foo'}

**Dictionaries**强制把key转换成string

    echo {'a': 1, 100: 'foo',}['a']
    echo {'a': 1, 100: 'foo',}[100]
    echo {'a': 1, 100: 'foo',}.a
    echo {'a': 1, 100: 'foo',}.100

**Dictionaries**索引

    let foo = {'a': 1}
    let foo.a = 100
    let foo.b = 200
    echo foo           "{'a': 100, 'b': 200}

**Dictionaries**赋值和添加可以使用相同的方式

    let test = remove(foo, 'a')
    unlet foo.b
    echo foo                    "{}
    echo test                   "100

**Dictionaries**有两种方式删除成员，remove会返回删除的value

### 35. Dictionary Functions

    echom get({'a': 100}, 'a', 'default') "100
    echom get({'a': 100}, 'b', 'default') "default

**get**函数

    echom has_key({'a': 100}, 'a') "1
    echom has_key({'a': 100}, 'b') "0

**has_key**函数

    echo items({'a': 100, 'b': 200}) "[['a', 100], ['b', 200]]

**items**函数

### 36. Move to Window

    echo winnr([{arg}])

如果不加参数，返回当前**Window**的编号，参数是**'$'**时，返回最后一个**Window**的编号，也即窗口的数量，参数是**'#'**时，返回上一次访问的**Window**的编号

    [count]winc[md] {arg}

相当于执行CTRL-W [count] {arg}，其中count表示从左上到右下排序的窗口的编号，当参数是w时，表示跳转到对应编号的窗口

### 37. Functions as Variables

    let MyFunc = function("append")
    echo MyFunc([1, 2], 3)                            "[1, 2, 3]
    let funcs = [function("append"), function("pop")]
    echo funcs[1]([1, 2, 3], 1)                       "[1, 3]

变量可以作为函数的引用，这时变量名必须大写。函数引用也可以作为**List**成员，这时变量名称不用必须大写

    function! Mapped(fn, l)
        let new_list = deepcopy(a:l)
        call map(new_list, string(a:fn) . '(v:val)')
        return new_list
    endfunction
    let mylist = [[1, 2], [3, 4]]
    echo Mapped(function("Reversed"), mylist)        "[[2, 1], [4, 3]]

**map**函数形如：map({expr}, {string})
**{expr}**是**List**或者**Dictionary**，使用**{string}**替换**{expr}**中的每一项。在**{string}**中v:val表示当前项的value，对于**Dictionary**，v:key表示当前项的key，而对于**List**，v:key表示当前项的索引。

    call map(mylist, '"> " . v:val . " <"')

在mylist中的每一项头部添加"> "，尾部添加" <"字符串。
**{string}**是一个表达式的结果，然后又会把这个结果再作为一个表达式

### 38. Absolute Paths

    echo expand('%')

**%**表示当前文件的相对路径

    echo expand('%:p')

**%:p**表示当前文件的绝对路径

    echom fnamemodify('foo.txt', ':p')

获取foo.txt在当前目录下的绝对路径，不管foo.txt是否存在

### 39. Listing Files

    echo globpath('.', '*')

显示当前目录下文件

    echo split(globpath('.', '*.txt'), '\n')

可以使用通配符

    echo split(globpath('.', '**'), '\n')

递归显示当前目录文件

### 40. Basic Layout

    ~/.vim/colors/

这个目录下包含了配色方案，如果运行了**color mycolors**，那么**~/.vim/color/mycolors.vim**会被执行。这个文件应该包含所有生成配色方案的vimscript命令

    ~/.vim/plugin/

这个目录文件在每次运行vim时都会执行

    ~/.vim/ftdetect/

这个目录文件在每次运行vim时都会执行，这个目录中文件应该设置自动执行命令。这些命令检查和设置文件的**filetype**，其它不应该做。这就意味着这个文件的内容最多一两行

    ~/.vim/ftplugin/

当设置**filetype**时，这个目录下相应的文件会被执行，比如执行**set filetype=derp**，那么vim会去查找和执行**~/.vim/ftplugin/derp.vim**或者**~/.vim/ftplugin/derp/**目录下的**\*.vim**文件。因为这些文件是在设置**buffer**的**filetype**时被执行的，所以它们只应该设置**buffer-local options**

    ~/.vim/indent/

类似于ftplugin下的文件。加载时也是只加载名字对应的文件。indent文件应该设置跟对应文件类型相关的缩进，而且这些设置应该是**buffer-local**的

    ~/.vim/compiler/

类似于indent文件。它们应该设置同类型名的当前缓冲区下的编译器相关选项

    ~/.vim/after/

这个文件夹下的文件会在每次Vim启动的时候加载， 不过是在**~/.vim/plugin/**下的文件加载了之后

    ~/.vim/autoload/

**autoload**是一种延迟插件代码到需要时才加载的方法

    ~/.vim/doc/

放置插件文档的地方
