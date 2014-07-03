---
layout: post
title: "Linux assembly(AT&T)"
date: 2014-04-13 14:00
category: "Kernel"
tags: linux kernel
---
{% include JB/setup %}

Linux assembly(AT&T)（未完）

------

### 对Linux Kernel的学习走起。书中刚开始介绍了bootloader启动Kernel的常识，之后就是托管给Kernel去自生自灭，其中启动的代码主要是汇编语言实现。
Linux下的汇编使用AT&T语法，与Intel的语法有所区别，搜索了一些AT&T汇编的用法（[Linux 汇编语言开发指南][1]，[汇编与C之间的关系][2]）：

#### 1. 在AT&T汇编格式中，寄存器名要加上'%'作为前缀；而在Intel汇编格式中，寄存器名不需要加前缀，例如：

AT&T格式   |Intel格式
-----------|----------
pushl %eax |pushl %eax

#### 2. 在AT&T汇编格式中，用'$'前缀表示一个立即操作数；而在Intel汇编格式中，立即数的表示不用带任何前缀，例如：

AT&T格式|Intel格式
--------|---------
pushl $1|pushl 1

#### 3. AT&T和Intel汇编格式中的源操作数和目标操作数的位置正好相反。在Intel汇编格式中，目标操作数在源操作数的左边；而在AT&T汇编格式中，目标操作数在源操作数的右边，例如：

AT&T格式      |Intel格式
--------------|----------
addl $1, %eax |add eax, 1

#### 4. 在AT&T汇编格式中，操作数的字长由操作符的最后一个字母决定，后缀'b'、'w'、'l'分别表示操作数为字节（byte，8 比特）、字（word，16 比特）和长字（long，32比特）；而在Intel汇编格式中，操作数的字长是用 "byte ptr" 和 "word ptr" 等前缀来表示的，例如：

AT&T格式      |Intel格式
--------------|--------------------
movb val, %al |mov al, byte ptr val

#### 5. 在AT&T汇编格式中，绝对转移和调用指令（jump/call）的操作数前要加上'*'作为前缀，而在Intel汇编格式中则不需要

#### 6. 远程转移指令和远程子调用指令的操作码，在AT&T汇编格式中为 "ljump" 和 "lcall"，而在Intel汇编格式中则为"jmp far"和"call far"，即：

AT&T格式                |Intel格式
------------------------|-----------------------
ljump $section, $offset |jmp far section:offset
lcall $section, $offset |call far section:offset

与之相应的远程返回指令则为：

AT&T格式           |Intel格式
-------------------|-----------------------
lret $stack_adjust |ret far stack_adjust

#### 7. 在 AT&T 汇编格式中，内存操作数的寻址方式为：

<pre>section:disp(base, index, scale)</pre>

而在 Intel 汇编格式中，内存操作数的寻址方式为：

<pre>section:[base + index*scale + disp]</pre>

由于 Linux 工作在保护模式下，用的是 32 位线性地址，所以在计算地址时不用考虑段基址和偏移量，而是采用如下的地址计算方法：

<pre>disp + base + index * scale</pre>

下面是一些内存操作数的例子：

AT&T格式                       |Intel格式
-------------------------------|-----------------------------
movl -4(%ebp), %eax            |mov eax, [ebp - 4]
movl array(, %eax, 4), %eax    |mov eax, [eax*4 + array]
movw array(%ebx, %eax, 4), %cx |mov cx, [ebx + 4*eax + array]
movb $4, %fs:(%eax)            |mov fs:eax, 4

### 内联汇编的用法
通常 C 代码中的内联汇编需要和C的变量建立关联，需要用到完整的内联汇编格式为：

<pre>
__asm__(assembler template 
	: output operands                  /* optional */
	: input operands                   /* optional */
	: list of clobbered registers      /* optional */
	);
</pre>

这种格式由四部分组成，第一部分是汇编指令，第二部分和第三部分是约束条件，第二部分指示汇编指令的运算结果要输出到哪些C操作数中，C操作数应该是左值表达式，第三部分指示汇编指令需要从哪些C操作数获得输入，第四部分是在汇编指令中被修改过的寄存器列表，指示编译器哪些寄存器的值在执行这条__asm__语句时会改变。后三个部分都是可选的，如果有就填写，没有就空着只写个:号。

[1]:https://www.ibm.com/developerworks/cn/linux/l-assembly/
[2]:http://learn.akae.cn/media/ch19s05.html
