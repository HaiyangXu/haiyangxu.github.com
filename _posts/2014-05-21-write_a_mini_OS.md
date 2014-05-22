---
layout: post
title: "超级mini的bootloader(OS)"
description: ""
categories: ["基础知识","Linux"]
tags: ["Bootloader","操作系统","Linux"]
published: true
---
{% include JB/setup %}

说了不能在瞎折腾的！下周一定不要再折腾了。昨天手贱，看到了一个自己写一个超级简单的os，其实就是写了一个简单的bootloader，只是输出了几个字符。链接在[这里][1]，不过我觉得它启动那部分搞复杂了，代码虽然借用的他的，不过我这里说的更简单粗暴，直观易懂。下面我简单说一下理解和把玩一下这个mini OS需要的知识和工具，这个真的不复杂也不难。我觉得写一个玩一下的操作系统不难，写一个能用的就难啦 哈哈。有时间我就把上面那篇文章翻译到这篇文章里面，现在就说一下大概吧，反正很简单。

##基础知识
 
 了解一点汇编，了解计算机启动过程。
 计算机启动过程recap：
 

 - 加电：计算机启动并从ROM读取和执行BIOS代码
 - BIOS寻找各种磁盘文件，例如软盘、光盘、硬盘等
 - BIOS从第一个找到的可启动盘上读取512 Byte的bootsector ，并开始执行
 - 这个512Byte的程序开始load操作系统或者一个更复杂的bootloader

这里就是之写了这个512Byte的程序，虽然只有512Byte，但它已经从BIOS接管了计算机了，所以可以说是一个超级mini的OS了。
关于汇编的知识就不赘述了。

##Mini OS

下面是这个mini os的汇编代码，代码总共只有30行左右，简单吧！

<script src="https://gist.github.com/HaiyangXu/b06c58a4d26b97fbd3f2.js"></script>

上面的代码已经注释了，下面详细解释一下.

##原理分析

首先是上面哪个`mov ax, 07C0h`这个地方是怎么回事呢？虽然我们这个bootloader它是在磁盘的512Byte但是我们要把它load进RAM,在BIOS把bootsector的512Byte加载进RAM的时候，CPU和BIOS协作早就把BIOS给加载进RAM了，所以这个时候RAM里面已经有内容了，所以在把bootsector加载进RAM的时候，放在从07C00H这个地址开始的空间。那么07C00H前面的空间就是BIOS的了，这里放了各种BIOS的子程序、例如后面的`int 10h`等中断调用。

也就是说，我们的mini OS被加载到RAM的时候，是在07C00H 到 07C00H+512的地址空间中。那为什么是`mov ax, 07C0h`呢 ？因为寻址的时候使用的(段地址*16+偏移地址)来计算的，即DS:OFFSET.一个段为16Byte，计算如下所示：

          DS:  07C0H   0000 0111 1100 0000 
    + OFFSET:   0000H       0000 0000 0000 0000
    =          07C00H  0000 0111 1100 0000 0000
    
然后加上4KB的Stack空间，（4096+512）/16=288 这个时候要在RAM上分4KB的配堆栈空间，要把SS，SP设置好，SS：堆栈的段地址，SP：堆栈指针，即当前位置。DS是数据段地址，就是我们这个程序开始的地址了。

    +-------------------+ <-- 07C0:0000, where the BIOS loads the boot sector
    | 512 bytes of code |
    +-------------------+ <-- SS:0000
    |   4KB of stack    |
    +-------------------+ <-- SS:4096 = SS:SP

所以SS=07C0H+288 ，SP=4096.
设置DS=07C0H,为什么呢？因为取数据的时候DS是我们这个mini OS的段地址，而后面的
**lodsb**就是把DS:SI指向的存储单元中的数据装入AL.

`mov si,text_string` 把text_string的地址偏移地址装入SI中。
`call print_string` 就是调用打印子程序了。
`jump $`就是个死循环。。。就是跳到当前语句的位置，当前语句的指令又对CPU说:来，跳到现在这个位置。。。

后面的print_string就是一个用`int 10H`中断输出字符的子程序。

`times 510-($-$$) db 0` 一个美元符号表示当前位置，两个美元符号表示当前段位置,`$-$$`就是到目前为止这个程序用掉的字节数，times就是用0填满510个byte,最后留了2个Byte给最后一句。
`dw 0xAA55`	0xAA55是约定了boot签名，就是说BIOS判断存储器上第一个扇区的,511 512 字节是不是0xAA55是的就认为它是bootloader，就加载进RAM。

这部分参考了很多资料，例如

http://stackoverflow.com/questions/3231607/stack-segment-in-the-mikeos-bootloader

http://www.cnblogs.com/awpatp/archive/2010/07/07/1772725.html

##从Mini OS启动




http://superuser.com/questions/514003/how-do-you-write-a-bootloader-to-the-mbr
 


> Written with [StackEdit](https://stackedit.io/).


  [1]: http://mikeos.berlios.de/write-your-own-os.html