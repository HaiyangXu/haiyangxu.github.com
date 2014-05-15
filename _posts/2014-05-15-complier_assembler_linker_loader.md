---
layout: post
title: "编译器、汇编器、连接器和加载器: 一个简要的介绍"
description: "description"
categories: ["基础知识"]
tags: ["编译器","汇编器","连接器","加载器","进程空间"]
published: true
---
{% include JB/setup %} 

本来想自己写一篇关于可执行程序的连接及程序执行时进程空间等问题的博客，结果被我发现了一篇好文章，遂翻译出来顺便学习。原文[COMPILER, ASSEMBLER, LINKER AND LOADER:A BRIEF STORY][1]，翻译主要还是更具自己的理解来，如有疑问请参考原文。

注意：
本文提供了一个进程（运行程序）相当多的细节故事。它试图探讨C/C++源代码如何预处理，编译，链接和加载为一个正在运行的程序。它是基于GCC（GNU编译器集合）来说明的。当你使用IDE（集成开发环境）编译器，如微软的Visual C + +，Borland的C++ Builder等，这里讨论过程对IDE用户来说是透明的。关于Linux的GNU GCC，G++，GDB等工具的命令和示例请参考这里[待添加]。

C编译器的能力：
能理解和预处理过程，编译，连接，加载和运行的C / C + +程序。

##1. 编译器，汇编器和连接器

通常情况下，C程序的构建过程包括四个阶段，分别采用不同的“工具”，如预处理器，编译器，汇编器和链接器。在最后应该有一个单一的可执行文件。下面就是这几个阶段（忽略了操作系统/编译器差异）发生的顺序。表1和图1展示了这几个阶段。

 - **预处理**是任何C编译的第一遍。它处理头文件，条件编译指令和宏。
 - **编译**是第二遍。它需要预处理器的输出、源代码，并生成汇编源代码。
 - **汇编**它采用汇编源代码，并生成带偏移地址的汇编清单。汇编器的输出存储在目标文件。
 - **链接**是编译的最后阶段。这需要一个或多个目标文件或库作为输入，并结合他们产生一种单一的（通常是可执行文件）文件。在这样做时，它解析引用外部符号，分配给最后地址的程序/函数和变量，并修改代码和数据，以反映新的地址（一个称为重定位的处理）。

如果你使用IDE进行开发，这些过程对你来说就是透明的。现在，我们将研究发生在链接阶段之前和之后过程中的更多细节。对于任何给定的输入文件，文件名后缀（文件扩展名）确定由什么样的编译工具来完成，下表给出了GCC的例子,关于这方面可以参考[GCC的文档][2]。在UNIX / Linux上，该可执行文件或二进制文件没有扩展名，而在Windows中的可执行文件，可能有.exe，.com和.dll的后缀。

![文件后缀和对应的处理方式][3]

下面的一张图给出了参与构建C程序，从编译开始，直到加载可执行文件成为内存中运行的程序（一个进程）的步骤，关于这些步骤的具体情况会在下面进行说明。

![从源代码到进程][4]

##2.目标文件和可执行

 - 在源代码被汇编后，会得到很多目标文件(Object File ,如 .o .obj ) ，然后通过连接得到一个可执行的文件。
 - 可目标文件和可执行文件的格式有好多种，例如像ELF(Executable and Linking Forma)和COFF(Common Object-File Format). ELF是Linux系统中使用的，而windows使用的是COFF ，下表给出了一些可执行文件格式的描述：
 
![enter image description here][5]
 

 - 目标文件的里的内容有段区域。这些段区域可容纳可执行代码，数据，动态链接信息，调试数据，符号表，重定位信息，注释，字符串表，以及附注。
 - 部分段会被加载到进程映像，另外一些段会提供一些必要的信息来帮助构建进程，同时还有一些仅用于连接目标文件。
 - 有几个部分是所有可执行文件格式共同的（可能有不同的名称，这取决于编译器/链接）如下表所示：

![enter image description here][6]




  [1]: http://www.tenouk.com/ModuleW.html
  [2]: https://gcc.gnu.org/onlinedocs/gcc/Overall-Options.html
  [3]: https://docs.google.com/drawings/d/1X6_Udtq8LfMkEGCl25e-lVqYJxF16FlkXNIUrz97rS0/pub?w=626&h=415
  [4]: https://docs.google.com/drawings/d/1jB91FZlTdaDIx53eaKQdtr51jqJVoqZ69yDaujrXZis/pub?w=405
  [5]: https://docs.google.com/drawings/d/12g71dluRSnqBovG-PdLDZZ0F3psmLl3_Q8xvcS1gUDg/pub?w=625
  [6]: https://docs.google.com/drawings/d/1VuKxrQA2DrD2NUDOQ4V4aPzjqVI-PtO3gRdeNvqTPBk/pub?w=625