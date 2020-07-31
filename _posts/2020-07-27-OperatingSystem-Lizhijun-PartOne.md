---
title: '哈工大李治军老师讲操作系统 --（一）操作系统基础'
category: Operating System
tags: 
  - 'OS-Lizhijun'
---

这份笔记是我在 3 月 23 号至 5 月 13 号，看哈尔滨工业大学的李治军老师讲《操作系统》这门课的视频所记（时间跨度共 52 天，中间陆续花了 24 天在看这个视频，并记录笔记，其余的 28 天因其它一些事而搁置了学习这个课程的进度）。当时主要是想重新复习下操作系统这门非常重要的基础课程，但整个所花费的时间，比我预期的要长不少。

这个课程的视频在 [中国大学MOOC](https://www.icourse163.org/course/HIT-1002531008?tid=1450346461) 和 [B站](https://www.bilibili.com/video/BV1d4411v7u7) 上都能找到。在看的过程中，了解到这个视频的录制时间在 2015 年。由于李老师在讲这门课时，把 32 讲的课程内容划分成 4 个类别：操作系统基础、进程与线程、内存管理和设备驱动与文件系统。因此，我也把这份笔记拆成 4 个部分，这是第一部分，操作系统基础。

<!-- more -->

这份笔记的内容主要由这门课程的 PPT 截图和李治军老师的讲解组成（当时在看视频时，把李老师的一些观点或讲解的内容，记录成文字，用来方便今后回顾）。李老师的这门课，通过结合 Linux 0.11 内核的关键代码来讲解操作性系统中的概念。

这个课程还配套了 8 个实验。李老师在这门课中，多次强调一定要去做这 8 个实验，Learn OS concepts by coding them，只有在实际中动手去做了，才能理解得更深刻，才可以说是真正学明白了操作系统。所以，如果自学的时间够的话，建议去做下这 8 个相关的实验（关于实验部分的内容，在这个笔记的附录中有给出链接）。

此外，学习李老师的这门操作系统课程最好有一些 C 语言（汇编的话，有一些基础更好，前面讲操作系统的启动会涉及较多，后面的课程内容中涉及较少）、计算机组成原理，数据结构的基础。有了这些前置知识后，我认为还要做好啃硬骨头的准备。因为操作系统它本身是一个系统，一些部分的实现较复杂，在学习的过程中需要在头脑中形成清晰的概念。

我想这也是为什么李老师说，只有把那 8 个实验做出来了，才算真的学明白操作系统了。因为只有当你在头脑中有了清晰的概念后，你才能在 Linux 0.11 的内核中进行改动，按照实验要求编写出那些代码。也就是说，如果你能编出来，那么在头脑中肯定对相关内容形成了清晰的概念。



&nbsp;

## L1 什么是操作系统

建立操作系统的宏观轮廓。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L1-ComponentofComputer.JPG" alt="L1-ComponentofComputer" style="zoom:60%;" />



用计算机帮助人们解决一些实际问题。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L1-ComputerSolvesProblem.JPG" alt="L1-ComputerSolvesProblem" style="zoom:60%;" />



在计算机硬件之上包了一层软件，让我们使用计算机更加方便，这就是操作系统。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L1-OSoverHardware.JPG" alt="L1-OSoverHardware" style="zoom:60%;" />



管理网络、电源和多核部分这个课程不讲，我们课程主要讲这 5 个最基本的部分。**一旦把这 5 个基本部分弄明白了，一台单CPU的、单机操作系统是如何管理这些硬件的，一个完整的操作系统就建立起来了**。有了这个完整的操作系统，再延伸就可以做网络、分布式、多 CPU 等内容。

高级操作系统课程

对于我们这个课程来说的话，就要把一台单 CPU、单机上的计算机操作系统，是如何管理这些基本的硬件，把它讲明白、讲透彻。这就是我们讲课的主要领域。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L1-DefinitionofOS.JPG" alt="L1-DefinitionofOS" style="zoom:60%;" />



课程目标：大家能够编写操作系统，如果这个目标太高的话，**大家能够改动操作系统，至少能够扩充操作系统**。比如说，有个新的设备，你需要给它开发驱动，并且提供向上的接口。

当然还有些学校更狠，贯穿了这三个层次。首先给你一个硬件的板子，你通过硬件手册在上面配一个操作系统，并且适当的提供接口，来提供给上层使用。CMU 就采用这种层次，完全是由一块板子一点点往上搭操作系统，这个要求是非常高的。**我们尽量集中在动操作系统，改操作系统**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L1-DifferentWaysofLearningOS.JPG" alt="L1-DifferentWaysofLearningOS" style="zoom:60%;" />



能改操作系统，要知道操作系统背后发生的事情，更需要知道这些事情是怎么用代码实现的。

8 个实验在课程中反复强调，这 8 个实验大家一定要认真做，做完这 8 个实验后，真正可以做到在操作系统上可以动手开始实践。如果在这 8 个实验的基础上，你再能做一些大型的实际项目，那你就真是达到我们课程的目标了：能够设计操作系统的一些模块，能够改操作系统。

**操作系统就是计算机的关键技术**，如果你只知道原理，你都不知道计算机内部发生了什么，你怎么能叫理解它的关键技术呢？怎么能叫掌握它的关键技术呢？怎么能叫掌控一个操作系统呢？

我认为虽然课程有些难，确切地说难，但是大家应该努力成为达到这一标准的人才——**掌握计算机的关键技术、核心技术**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L1-theGoalofThisCourse.JPG" alt="L1-theGoalofThisCourse" style="zoom:60%;" />



我们应该跟得上潮流，跟世界去接轨，不敢说超越他们，至少我们在向他们靠近，这我觉得是应该的。

斯坦福怎么学操作系统的呢？他们做四个实验，这四个实验可能现在你们还看不懂，我希望学到后面的时候，你们再回来看看这四个实验。**他们这四个实验要动实际操作系统中，四个非常关键的部分**，那真是在操作系统里面大动干戈。

那么，如果对操作系统不理解，你不进去它，只是知道一些皮毛的原理，要想动这四个干戈，几乎是完全不可能。所以就是说，斯坦福的学生在学完操作系统后能做到这些，他们对内部一定是非常理解。这些学生将来具有系统的能力，具有控制操作系统的能力，具有控制整个计算机系统的能力，具有设计系统的能力和实现系统的能力。这个能力对计算机行业来说，是非常重要的。

如果没有能够进去操作系统，那么你永远也不能说你具有控制计算机系统的能力。而 CMU 更狠，给你个板子，自己做一个系统出来，非常难，非常复杂。只有经过这样的训练，在计算机行业才能堪当大任，承担系统工作。所以老师觉得，在这门课程中我们有必要深入进去，在操作系统里面明白操作系统背后发生的事情，在操作系统里面真正能够做一些事情。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L1-WhatIsTheTopUniversityStudentsLearningInThisCourse.JPG" alt="L1-WhatIsTheTopUniversityStudentsLearningInThisCourse" style="zoom:60%;" />



**要知道原理，马上浮现代码；要看到代码，懂得背后的原理所在。要在这两个方面来回的切换。所以，这课程是对编程能力和抽象能力，问题理解和问题求解都要求非常高**。大家要打起精神，因为我觉得只有这样做了，你才能真正的明白一个真实的操作系统，明白这样的真实操作系统后，才为将来在系统中能做一些工作、提升自己的系统能力奠定一个基础。当然要做到这一事情，要动手、动脑、要思考。是有一定难度，所以每次在课程中时我都用一句话来激励大家——绝知此事要躬行。

**如果没有躬行的态度，没有躬下身来深入到操作系统内部的决心，那你不会学好这门课，你也不能真正去控制，提高自己的系统能力，没有真正的能力去控制一个操作系统**。所以大家一定要带躬行的态度去做到这件事情。

而我前面也说了，只有真正深入到操作系统的内部，我们才能建立系统的能力，才能和国际接轨，我们的学生才能够掌握计算机的核心技术，才有机会和国际的同行业的人去进行同等的竞争。从下节课开始，我们就开始一步步地进入系统。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L1-LearningOSConceptsbyCodingThem.JPG" alt="L1-LearningOSConceptsbyCodingThem" style="zoom:60%;" />



&nbsp;

## L2 揭开钢琴的盖子

对于我们课程来说，就是要打开操作系统，进入操作系统的内部，来看操作系统到底发生了什么。你只有进入了内部，才能明白操作系统到底是什么，将来才能有能力、有机会在这里面去做一些事情。



这神秘的开机背景的黑色后面到底发生了什么事情？老师认为这是揭开盖子的第一步。而且因为它是打开电脑后看到的第一幅画面，所以从这里开始合情合理。实际上从这个地方，我们安排了一次实验——实验 1，在开机的时候不仅能看到这个画面，我们希望通过程序能控制这个画面。

控制开机的系统，这是实验 1 的内容，在本次课的时候我们会讲一部分如何控制，大家到时候就可以编程序去控制。我们上节课也说过了「绝知此事要躬行」，要自己进去去做操作系统，要 coding them，才有可能把操作系统学得更加深刻，更加有用。

**思考要借助于材料，不可能坐着乱想一气，那不叫思考，思考要借助于常识，知识和推理**。

计算机是怎么工作的，知道计算机工作的原理，对计算机行业的来说这是最最基本，也是最最重要的常识。这个常识非常重要，**知道计算机怎么工作对整个操作系统的理解都是非常重要**，很多时候都是从这个最基本的原理开始，一步一步地思索，推理，我们就明白了，oh，最终它就应该是那个样子。

当你把思索中应该的样子再写成程序，那就是操作系统。大家要慢慢去体会刚才老师所说的话。**头脑中形成这样的一个思索的过程，那么才能把它变成程序，操作系统才可能编得出来**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L2-InitialPictureinTurningonComputer.JPG" alt="L2-InitialPictureinTurningonComputer" style="zoom:60%;" />



计算机是在一个模型下设计出来的，所以这个模型必须得知道，也就是说**计算机是计算模型的一种实现**。

图灵机，这是非常基础的一个常识。那么图灵是怎么定义这个图灵机的呢？怎么定义这个计算模型的呢？怎么想到的呢？实际上也很自然，他借鉴了人在纸上计算的这一过程，将这个过程模拟出来，那么实际上就是一个计算机。

把计算出 `3 + 2 = 5` 的这一过程，用一个自动化设备把它模拟出来，用一条纸带来模拟这个纸，用控制器去模拟大脑，用读写头去模拟眼睛和笔。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L2-TuringMachine.JPG" alt="L2-TuringMachine" style="zoom:60%;" />



图灵机还要再一步演化。从图灵机到通用图灵机这也是图灵提出来的，一个非常简单但是也非常伟大的一个推进。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L2-FromTuringMachinetoUniversalTuringMachine.JPG" alt="L2-FromTuringMachinetoUniversalTuringMachine" style="zoom:60%;" />



冯 · 诺依曼提出**如果能把程序存到内存里，然后把程序载入到控制器里，然后进行解释、执行，之后就可以输出结果**。

IP (Instruction pointer)；PC (program counter)

**计算机的核心结构就是这样一张图，这张图在我们整个操作系统，整个计算机中是非常非常基本的一个常识**。首先把程序放到存储器里，然后用一个指针指向它，接着取指、执行，取指、执行... ，计算机就开始工作，而且取指、执行还是自动的，不断地取指、执行来产生结果。**所以计算机是怎么工作的，就四个字「取指执行」**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L2-fromUniversalTuringMachinetoComputer.JPG" alt="L2-fromUniversalTuringMachinetoComputer" style="zoom:60%;" />



打开电源，计算机执行的第一句指令是什么：`PC = ?` 

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L2-WhatIsTheFirstInstructionExecutedinTurningonComputer.JPG" alt="L2-WhatIsTheFirstInstructionExecutedinTurningonComputer" style="zoom:60%;" />



对于 Intel PC 机来说的话，内存中有一部分是固化的，叫 ROM BIOS (Basic Input Output System) 。

引导扇区。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L2-ROM_BIOS+BootSector.JPG" alt="L2-ROM_BIOS+BootSector" style="zoom:60%;"/>



这 512 个字节是什么东西呢？里面写了什么内容呢？做些什么事呢？如果让我写，我能写出来吗？**操作系统所有的故事都是从这里开始**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L2-TheCodeinAddress0x7c00.JPG" alt="L2-TheCodeinAddress0x7c00" style="zoom:60%;" />



**引导扇区的代码是一段汇编代码**，为什么要做成一段汇编代码呢？为什么不用 C 写呢？写的 C 程序将来都是要经过编译的，C 程序经过编译以后会产生一些你不知道或没法控制的东西。比如说 C 语言中定义的变量 `int i;` ，你不能控制变量 i 经编译以后在内存中的哪个位置。

而汇编不一样，汇编中的每一条指令最后都变成了真正的机器指令，所以你可以对它进行完整的控制。而在引导扇区，你当然要对它进行完整的控制，绝对不能有任何出入的控制。实际上在操作系统中有很多这样的控制，**所以在操作系统中有很多这样的汇编**。这也正好回顾了对应着刚才那句话，操作系统对汇编要求很高。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L2-bootsect.s.JPG" alt="L2-bootsect.s" style="zoom:60%;" />



这个代码稍微略略地讲，因为代码太多了，我们讲些重点就可以了。**大家在学习的时候也肯定是掐住重点**，因为操作系统代码要是重头看到尾，看到一遍，你肯定最后都不知道在哪里。**因为太过于庞杂，所以一定要抓住主线来学习操作系统才可以**。

通过我们这个课程，大家就可以抓到操作系统的主线是什么？原理是什么？对应的代码中的主线是什么？骨干是什么？只有这个骨干在头脑中立起来，你才有可能编出操作系统。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L2-jmpi_go_INITSEG.JPG" alt="L2-jmpi_go_INITSEG" style="zoom:60%;" />



把 setup 读进来，再把操作系统的其它部分代码读进来。

更改开机启动时，屏幕上输出的字符，例如 "LiOS is loading"，这就是我们实验 1 的一个部分。还是前面那句话，不 coding 操作系统，不会学会操作系统。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L2-ok_load_setup.JPG" alt="L2-ok_load_setup" style="zoom:60%;" />



引导扇区的程序执行结束，开始跳到 setup 去执行。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L2-read_it-ReadSystemModule.JPG" alt="L2-read_it-ReadSystemModule" style="zoom:60%;" />



&nbsp;

## L3 操作系统启动

上一节我们讲了引导扇区，引导扇区是操作系统的第一个部分。

在计算机刚一开始上电的时候，操作系统在硬盘里。而计算机的工作原理是取指执行，**要想取指执行就必须得将代码放到内存里**。也就是说，只有将代码放到内存里，才能够进行取指执行，才能够让计算机完成我们想要的工作。

刚一开始操作系统在磁盘上，**所以要做的第一部分工作就是，一定要把操作系统从磁盘上载入到内存里。不载入到内存里，就没有办法进行取指执行。也就是说，厨师就看不到操作系统的这个菜谱**。

bootsect.s ：将磁盘上的操作系统，读入到内存里（分段读）。bootsect.s 之后执行 setup.s 。



**实际上开机就做了两件事，第一件事读入操作系统（bootsect.s），第二件事就是 setup，立起来、初始化**。只有这样初始化以后，操作系统才能知道硬件长什么样，才能进行管理。

在后面执行起来的时候，**操作系统也一直会停留在 0 地址开始处，形成的格局是，在整个内存中从 0 地址开始一直是操作系统，再往上就是一些应用程序，例如 word、ppt、QQ 等**。

那么这就是 setup.s 做的一些事情，**它挪动下操作系统，让它从内存地址 0 开始，然后获取一些硬件参数，为将来操作系统能立起来，能初始化，能管理硬件做好准备**。

那么接下来 setup.s 的其它内容，老师在这里不再说了。大家感兴趣的话可以去看下我们这个课程的辅助教材《Linux内核完全剖析》0.11 版，我们这些代码主要都取自于 Linux 0.11。大家如果感兴趣的话可以拿那本书看下 setup.s 的其它内容，但都不是非常重要。而在我们课程中出现的代码是非常重要的，是整个操作系统的骨干代码。如果能够把课程中的代码全部学清楚，那么，形成的操作系统在脑中的样子就够了。如果把 Linux 0.11源码剖析的那本书配合着去阅读，那就更好。

所以 setup.s 在做完这件事后，接下来就要退出它的舞台了。而操作系统仍继续执行，setup.s 最后又做了这样一件事，也非常重要，**就是进入保护模式**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L3-setup.s.JPG" alt="L3-setup.s" style="zoom:60%;" />



setup.s 在最后的时候执行了这样一条指令 `jmpi 0, 8` 。这条指令在计算机里面，在整个操作系统里面，在我们的课程里面发挥非常重要的作用。能理解它，可以说对操作系统的认识就有了一定的层次。

**从这个地址开始，寻址的方式开始改变**，实际上也应该发生改变。这是 setup.s 所做的一件事，也是操作系统要立起来、计算机要立起来，要做的一件很重要的事。

**接下来要从 16 位机切换到 32 位机，要切换到 32 位模式，32 位模式也叫保护模式**。所以它要切换到保护模式进行工作，前面的 16 位模式已经不能满足需要了，所以在 setup.s 最后要做这样一件事，就是要切换到保护模式，要启动 32 位的寻址方式，这个时候内存就变大了。

cr0 寄存器的最后一位如果是 0，那么就是 16 位模式；如果是 1，那么它就是保护模式，当这个位置置 1 后就导致 CPU 要走另外一条解释执行的电路。而这条解释执行的电路要怎么来解释执行呢？这就是非常著名的概念—— gdt。

**gdt (Global Descriptor Table，全局描述表)，cs (选择子) 根据这个 table 选出其中一个表项，根据这个表项产生基址，再和 ip 偏移加在一起来产生 32 位的地址（这时 cs 和 ip 都是 32 位的寄存器，两个 32 位的寄存器加在一起仍是 32 位）**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L3-jmpi_0_8.JPG" alt="L3-jmpi_0_8" style="zoom:60%;" />



<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L3-GDT-IDT.JPG" alt="L3-GDT-IDT" style="zoom:60%;" />



<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L3-InitialGDT+IDT.JPG" alt="L3-InitialGDT+IDT" style="zoom:60%;" />



<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L3-SelectGDTItemandUseIPRegistertoGetMemoryAddress.JPG" alt="L3-SelectGDTItemandUseIPRegistertoGetMemoryAddress" style="zoom:60%;" />



**setup.s 到底做了些什么事呢**：

- 读了些硬件参数，为了将来建立操作系统来打下基础；
- 把 system 模块挪动一下，挪到 0x0000 地址处，将来操作系统的核心代码就一直存在那里；
- 启动了保护模式，应用了一条高级指令，32 位的汇编指令，用这条指令跳到了 0 地址处去执行。

&nbsp;

**BIOS 读 bootsect.s，bootsect.s 读 setup.s，setup.s 读 System 模块第一部分。这几个步骤要顺利执行，只要中途有一点儿错误就会死机，操作系统最大的错误就是死机**。

**编写操作系统，除了要写源码以外还要编写操作系统的控制代码，这就是著名的 Linux/Makefile** 。学完这个课程以后 Makefile 必须会，也就是在编大型软件的时候你要控制整个软件，让它合成一个结构就必须用 Makefile 。

**我们通常把一个操作系统最后编译出来的样子叫 Image (一个操作系统的镜像)，我们有一堆源码，这堆源码通过 Makefile 产生了操作系统的镜像**，这个镜像必须要长成这个样子。将这个镜像放在 0 磁道 0 扇区（可以通过一个工具把 Image 写到磁盘的任意位置），然后开机引导的时候，操作系统顺理成章地读了进来，被立了起来，待会儿会被初始化，然后会产生一个我们久违的桌面（shell）。有个这个桌面以后你就可以用操作系统，操作系统的启动就完成。



<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L3-JumptoSystemModule.JPG" alt="L3-JumptoSystemModule" style="zoom:60%;" />



操作系统是个复杂的东西，光汇编就用了三种。关于这三种汇编的语法我们这里也不能一一展开，到时候需要的时候，我们再简单地说一下，大家感兴趣的话可以查阅相关的资料。**但老师的原则是用到的时候再去看一看，掌握课程核心就行**。否则，等你把这三种汇编都学完了，可能这课你也就懒得去听了。所以用到的时候多去查阅查阅资料，多去找找答案就可以了。

**head.s 执行完跳出来后要去执行 main.c，main.c 是 C 函数，即从汇编要跳到 C函数。当 main 函数返回的时候，系统就进入死循环，操作系统就是一个永不停止的程序**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L3-head.s.JPG" alt="L3-head.s" style="zoom:60%;" />



<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L3-ThreeTypesAssemblyLanguage.JPG" alt="L3-ThreeTypesAssemblyLanguage" style="zoom:60%;" />



mem_init 函数执行内存的初始化

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L3-Jumptomain.c.JPG" alt="L3-Jumptomain.c" style="zoom:60%;" />



<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L3-main.c.JPG" alt="L3-main.c" style="zoom:60%;" />



<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L3-mem_init.JPG" alt="L3-mem_init" style="zoom:60%;" />



最后我们总结下上节课和这节课讲的内容。**我们讲了 bootsect.s，setup.s，head.s，main.c，以及 main.c 中的 mem_init () 函数，讲了这些后，关于操作系统的初始化基本完成**。讲了这么多内容，我们整理下它们到底做了些什么事情呢？实际上就是：

- bootsect.s 将操作系统从磁盘读进来；
- setup.s 获得了一些参数，启动了保护模式；
- head.s 初始化了一些 gdt 表、一些页面，然后再跳到 main.c ；
- main.c 里面是一堆类似 mem_init () 函数来初始化内存、硬盘等。

&nbsp;

把这一系列动作合在一起，实际上在系统启动的时候就做了两件事：**第一件事就是读入内存，即把操作系统读入内存；第二件事就是初始化**。

为什么要读入内存呢？**因为读入内存后，操作系统才能够进行取指执行，操作系统才能发挥它的功能**。

为什么要初始化呢？**因为操作系统要管理计算机的硬件设备，是管理计算机硬件的软件系统，要完成这个管理，我们就要对每一个硬件初始化一些数据结构**。

现在我们就明白了操作系统启动的大概过程。那么，这个过程明白了，对将来的学习会起很重要的作用。我们会在学习后面的内容时，再不时地回过头来看当时初始化的时候，初始化的内容。



&nbsp;

## L4 操作系统接口

操作系统的接口，实际上也是操作系统的第二部分。前面我们讲了系统的启动，实际上属于第一部分。在第一部分中，我们讲了启动是怎么回事，**最后实现的样子就是将操作系统的程序，读入到从内存 0 开始的地方**。也就是说，在系统启动的时候，将操作系统从磁盘载入到内存里。

除了将操作系统代码放到内存里，还创建了一些初始的数据结构，比如 GDT、IDT、mem_map 等。也就是说，在系统初始化的时候，**在操作系统引导部分，将操作系统从磁盘读入到内存中。现在内存中从 0 地址开始就有了操作系统，而且还有了一些重要的、初始化好的数据结构**。

从今天开始，我们学习从上层应用怎么穿到操作系统里面，从而最终怎么使用硬件。我们说过，这是我们一个比较重要的主题，从接口进来，来看操作系统到底发生了什么事，明白操作系统背后的原理以及通过这些原理，将来能真正地扩充系统的功能，能设计系统的模块。这是我们强调的系统能力。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L4-OS_Interface.JPG" alt="L4-OS_Interface" style="zoom:60%;" />



**操作系统的接口：连接上层应用和底下的操作系统，给出来的东西是很简单的东西，但是在内部要完成这种转换和屏蔽细节的功能**。

所以我们这里讲得是，给出来的东西是什么（很简单、很自然的东西），然后还要看这背后到底是怎么做成的。这就是我们讲接口这部分要完成的两项任务。今天我们讲前面一个，下一节再讲第二个。这两讲合在一起，就讲完了系统接口的完整故事。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L4-Interface.JPG" alt="L4-Interface" style="zoom:60%;" />



<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L4-WhatIsOSInterface.JPG" alt="L4-WhatIsOSInterface" style="zoom:60%;" />



<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L4-UserHowtoUseComputer.JPG" alt="L4-UserHowtoUseComputer" style="zoom:60%;" />



**所有的命令再复杂，比如 gcc 命令，但这个命令也是开发者编写的一段程序。所有的命令实际上都是一段程序，这个程序编译完以后会生成可执行的文件，然后再在命令行里敲入这些命令后执行**。

前面一部分我们讲开机、系统启动的时候，最后会打开一个桌面，对 Linux 来说的话，最后就是打开一个 shell。打开 shell 和打开桌面实际上是一回事，待会儿你马上会体会到。

shell 也是一段程序，可以看到这个程序的主体是一个死循环，所以 shell 是一个不停机的程序，它不断地等着用户敲入命令。

命令行是怎么回事呢？就是一堆程序，只是在程序中增加了一些重要的函数，来对计算机进行使用。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L4-CommandLine.JPG" alt="L4-CommandLine" style="zoom:60%;" />



**图形按钮是基于一套消息机制。我们讲的代码是基于 Linux 0.11，Linux 0.11 只有命令行，没有图形界面**。而这一套图形界面在新的 Linux，比如 Ubuntu 系统上有。Windows 系统也有，它著名的就是提供了一套图形界面机制。

我在这里要加一句题外话，大家完全可以在 Linux 0.11 上实现一套图形界面。实现图形界面就是要实现鼠标的点击，实现消息机制，再实现一个画图的功能。

&nbsp;

一个他操作系统课程上的学生：

他想在 Linux 0.11 上做一个图形界面，并且驱动一个鼠标来看一看。做这个实际项目当时老师非常鼓励，就和他们讨论了任务，需要做鼠标的驱动，要捕捉鼠标的中断，要做这样的消息机制，要开启图形模式，要在显示器上绘图，所谓绘图就是描一个个的图形像素。要这三件事，这个同学带着 4 个人的小组在两个月的时间做了出来，而且在 Linux 0.11 上做了一个用鼠标操作的游戏。

我的研究生，操作系统课程的助教，当看到他们这个小组在 Linux 0.11 上能做出这样一个东西，当时都非常惊讶。这个组的同学做完这个项目后非常有成就感。

实际上讲这个小故事的含义就为了说明，所谓的图形化界面并不复杂，并不神奇，无非是实现一些东西。要实现一个消息队列，当鼠标点下去后要通过中断放到系统内部的应用消息队列。而应用程序要写一个系统调用，GetMessage，要从内核里面、从操作系统里面把这些消息一个一个地拽上来。每拿出来一个消息，再对这个消息进行识别，执行一个相应的函数，来实现相应的功能。这个应用程序是一个不断从消息队列中取出消息的循环，这就是消息机制。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L4-PictureButton.JPG" alt="L4-PictureButton" style="zoom:60%;" />



无论是命令行还是图形界面，实际上都是一些程序，这些程序主体就是 C 程序。这个学完数据结构、C 语言都可以写出来。而里面关键是调用了一些重要的函数，正是调用一些重要的函数来进入操作系统，来使用硬件。

所以由此可以看出，计算机在上层应用是怎么使用底层的硬件的呢？就是普通的 C 语言，再加上关键性的重要函数。**所以什么是操作系统的接口呢？就是这些函数，这些函数就是操作系统给我们提供的接口**。

**什么是操作系统接口？就是系统调用**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L4-WhatIsOSInterface-FunctionProvidedbyOS.JPG" alt="L4-WhatIsOSInterface-FunctionProvidedbyOS" style="zoom:60%;" />



有哪些具体的操作系统接口呢？我们前面看到了 printf，printf 还不算操作系统接口，它最终调用了一个系统调用 write。既然我们的课程是要通过、穿过系统的接口，进入到系统的内部去学习。那么，你就必须知道有哪些接口。虽然我不要求大家背一背，有哪些系统调用，把它都记住，这没有必要。但是我们应该知道，哪些地方是系统调用，尤其是要知道去哪里查系统调用。

POSIX（Portable Operating System Interface of Unix），IEEE 制定的一个标准族。

**系统调用、操作系统接口应该做成统一的。POSIX 就是一个统一的接口，由 IEEE 定义的。如果一个操作系统能支持这个接口，那么以后使用起来将会很方便**。

**上层的应用程序，无非就是使用系统调用，再加一些别的代码**。那么，如果系统调用不变以后，别的代码可以在这个操作系统上跑，也可以在那个操作系统上跑。因为这些程序里面，除了用到这些函数（系统调用），其它的内容都是基本的 C 语言，而基本的 C 语言都是统一的。所以，基本的 C 语言，再加上不同的操作系统提供的相同的系统调用，那么应用程序是可以在不同操作系统上跑的。这样的话就很方便。

虽然不要求记住这些系统调用，这也不是我们课程的主题，但是应该知道去哪里查这些系统调用——POSIX。这是个标准，它遵循了 Unix，采用了 Unix 惯例（在操作系统名称中只要 X 出现，通常指的是 Unix）。

如果你对操作系统的系统调用比较关心，可以去查 POSIX 手册，Linux，Unix 操作系统都符合这个标准。POSIX 手册在网上能下载，也可以通过这个手册来看一看，一个操作系统应该给上层提供什么功能呢？提供什么接口呢？

那么，有了这个接口以后，你想想，如果你要设计一个操作系统，是不是也要提供这样一些基本的接口呢？这样的话，在 Linux，Unix 上开发的应用程序，在你设计的这个操作系统上也可以跑起来。

那么老师最后用一句话来总结：**什么是操作系统接口呢？就是一些函数，叫系统调用的函数。它具体来说就是 fork，open，write 等这些函数**。下一节我们再来看一看，这些函数在操作系统里面到底是怎么展开的？怎么就能进入系统？怎么就能实现它背后要求的功能？

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L4-SystemCall-POSIX.JPG" alt="L4-SystemCall-POSIX" style="zoom:60%;" />



&nbsp;

## L5 系统调用的实现

操作系统程序在内存中，应用程序也在内存中，应用程序想访问操作系统功能为什么不直接跳进去（jmp，mov 等），直接访问不行吗？不允许，**在内存中不能随意的调用内核的数据**，不能随意的 jmp 。

操作系统里面有很多很重要的东西，比如说 root 用户的用户名和密码，操作系统里面就很可能有。这样在输入用户名和密码登录的时候，操作系统才能在内存中去检验，输入的这个 root 用户的密码对不对。所以，一旦允许应用程序随意地访问操作系统内核，这个 root 用户的密码很可能就会被应用程序所掌握。

应用程序 word 中的内容，通过操作系统写到磁盘上。那么它写到磁盘上的有些字符串，在一定时间段内会在操作系统中留下来，留在缓冲区里。如果允许应用程序随意访问操作系统程序的话，那么它就可以把当前缓冲区里 word 应用程序的字符串给拿出来，那么就可以看到别人 word 里的内容，这样的话系统就太不安全了。

那么接下来我们要讲两件事情：**第一个，怎么才能禁止应用程序访问操作系统内核，都在内存中，凭什么不让应用程序随意去访问；第二个，如果这样的话，那么操作系统就应该提供一个门，即接口，给应用程序去调用**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L5-ForbidApplicationProgramtoAccessOScodeRandomly.JPG" alt="L5-ForbidApplicationProgramtoAccessOScodeRandomly" style="zoom:60%;" />



**通过硬件设计将应用程序和操作系统程序区分开来**，所以操作系统和硬件联系非常紧密。硬件把内存分隔成不同的区域，其中主要分成两个区域：**一个区域是用户态（用户段），另一个是内核态（内核段）**。

在前面我们分析汇编程序时，我们应该有个印象就是计算机对内存的使用都是一段一段的。**内核段的内存执行在内核态下，用户段的内存执行在用户态下**。

**内核段，用户段使用了段寄存器来进行区分**。有几个段寄存器有很重要的内容，分别是 CPL (Current Privilege Level)，RPL (Request Privilege Level) 和 DPL (Descriptor Privilege Level)，而 DPL 是用来描述你要访问的那个目标段的特权级别。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L5-KernelMode_UserMode-CPL-DPL.JPG" alt="L5-KernelMode_UserMode-CPL-DPL" style="zoom:60%;" />



**计算机提供了唯一的方法，就是通过中断指令 int 才能进入内核。什么是系统调用，实际上就是一段包含 int 指令的代码**。上层应用程序调用的接口，实际上函数展开后，包含一段含有 int 指令的代码。然后通过这个唯一的中断进入内核，进入内核以后操作系统写中断处理，获取想调用程序的编号，接着操作系统根据编号执行相应的代码。例如，表面上看是调用一个 open 函数，实际上背后发生了很多事情。

中断是进入内核的唯一方法。open 这个函数实际上就变成一段包含 int 的指令。**而要实现中断，操作会提供相应的中断处理函数**。在中断处理函数中，根据是 open，read，或 write 再跳去相应的地方去执行，这就是系统调用背后发生的故事。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L5-intInstruction-OnlyWaytoEnterOSKernel.JPG" alt="L5-intInstruction-OnlyWaytoEnterOSKernel" style="zoom:60%;" />



操作系统初始化规定 int 0x80 为中断指令。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L5-int0x80.JPG" alt="L5-int0x80" style="zoom:60%;" />



接下来我们再详细地看一看 int 0x80 到底做了些什么事。

总结一下就是，将一个系统调用号置给 eax，然后调用 int 0x80。调用 int 0x80 就到了内核，所以，这就解释清楚了系统调用核心的第一部分。因为进入内核的唯一方法就是靠 int 指令。write 通过一个宏展开的汇编，已经变成了一段内嵌的汇编代码，其中包含的核心语句就是 int 0x80。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L5-LinuxSystemCallDetails.JPG" alt="L5-LinuxSystemCallDetails" style="zoom:60%;" />



<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L5-SomeCodeinWriteFunction.JPG" alt="L5-SomeCodeinWriteFunction" style="zoom:60%;" />



int 0x80 表面上虽然是一句指令，但实际上这句指令做了很多事。int 0x80 要查 IDT 来确定中断去哪个位置执行。老师在这里插句题外话，**中断是计算机设计中一个里程碑的创新、发明创造。有了中断，可以做很多事。中断对操作系统、对计算机来说非常重要**。

那么中断是个什么东西呢？程序执行到中断就停下来，而跳去另外一个地方去执行。当在另一个地方程序执行完以后，再回来。

**所以 int 0x80 指令就是要去 IDT 里面去取出地址，跳到中断处理函数那里去执行。中断处理函数执行完后，再返回，执行下一条指令，把 eax 值赋给 res。也就是说，中断处理函数返回后，int 0x80 的指令就执行完了**。

int 0x80 用哪个中断处理函数去处理呢？这个是最关键的。

现在可以清楚为什么只能用 int 0x80 来进入内核了。因为进入内核的那一刻，在应用程序执行时 CPL 肯定等于 3，而系统调用展开成一段包含 int 0x80 的代码，而 int 0x80 要访问 IDT，在 IDT 中把 DPL 置成 3，此时 CPL = 3 并且 DPL = 3。**所以才能跳到 80 号中断，跳进去以后，用段和偏移设置新的 PC (cs:ip)，这时 cs = 8，ip = &system_call**。接着通过 GDT 找到 &system_call 相应的地址去执行。

而为什么 cs = 8 呢？**因为当设置为 8 后，cs 的最后两位为 0，因此 CPL = 0，然后就可以跳到内核代码 system_call 处去执行**。 

这个完整的过程就是：**在初始化时，80 号中断的 DPL 设置为 3，故意让 CPL = 3 的用户代码能进来。一旦进来执行后把 CPL 设置为 0，这时就变成了内核态，可以跳到任意的位置去执行程序。将来中断在返回的时候，会执行一条指令，这条指令执行完以后 cs 的最后两位又变成了 3，此时又变回了用户态**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L5-Execute_int0x80_process-IDT.JPG" alt="L5-Execute_int0x80_process-IDT" style="zoom:60%;" />



接下来我们来看一看 system_call 要干什么事情，

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L5-InteruptHandler-system_call.JPG" alt="L5-InteruptHandler-system_call" style="zoom:60%;" />



sys_write 是怎么实现的？在内部又发生了什么事？那必须要等到讲完文件读写、讲完 I/O 驱动才能讲，所以这个接口的故事到这里就可以了。

**我们说过，我们要通过接口深入到内核。现在我们讲清楚了在操作系统边界发生了什么事，而在内部发生了什么？那得讲完操作系统里面核心的东西，才能回答这个问题**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L5-_sys_call_table-write.JPG" alt="L5-_sys_call_table-write" style="zoom:60%;" />



最后我们再来说下前面的这个例子。我们从这个直观的例子开始，再从这个直观的例子结束。

现在我们讲明白接口了，从下次课开始我们就要深入到内核中去讲。但是我们在深入到内核中开始讲之前，我们要简单地讲一点操作系统的历史。希望通过这个历史来激发大家，也引导大家，通过历史来看一看，我们为什么要进入到内核中去学习，我们为什么使用这个套路。

根据前面的讲解，大家可以看到，我们这个课程的风格就是讲操作系统的内核到底是怎么做的？操作系统的接口是怎么弄的？操作系统是怎么初始化的？代码是怎么写的？我们通过历史来讲讲我们为什么要这样上这个课？然后，所有的事情准备好了以后，剩下的就一部分、一部分地从上层应用切进去。然后看内核到底是怎么编出来的？原理是什么？对应的代码是什么？为什么向上层去展现成那个样子？

我们配了很多实验，其中实验二就是让大家去实现 whoami()。一定要认真去做这些实验，这样后面的实验才能做，后面的内容也才能更好的理解。大家回去后去做实验二去吧。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L5-whoami-_system_call-sys_whoami.JPG" alt="L5-whoami-_system_call-sys_whoami" style="zoom:60%;" />



&nbsp;

## L6 操作系统历史

为了加深对操作系统的理解，我们特意增加了这一节，就是来看一下操作系统的历史。根据操作系统的历史，我们会对操作系统是怎么编出来的，操作系统为什么要有那些模块，什么东西才是它的核心，以及它在历史上是怎么产生出来的会有个了解。

**通过读操作系统的历史，也能让我们抓住操作系统的重点。这样在打开操作系统的时候，才不至于迷失方向。因为操作系统是一个非常复杂的东西，如果在头脑中没有一个对它清晰的图像，没有一个核心的骨干在里面的话，那么就容易懵了**。



实际上自计算机一出来就有了操作系统，因为人们认识到一个计算机如果不配上操作系统，使用起来的话就太费劲。在 IBM7094 上，**这时的操作系统不能算是操作系统，它就是一个监控程序**，这个监控程序很简单，只要发现第一个任务出错或完成了，它马上修改一下 PC 指针，让它去执行第二个任务，若第二个任务出错或完成，就切换到第三个任务，以此类推。

这时计算机没有任何额外的动作，也从来不停下来，就一直在那运行。当处理的任务出错时，它会把出错的信息输出到磁带上，给人们去分析。所以这个时候的操作系统非常简单，也算不上是操作系统。**但是，这是最开始的计算机，它和现在的计算机差别太大了，这是一个演变的过程**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L6-IBM7094-BatchSystem.JPG" alt="L6-IBM7094-BatchSystem" style="zoom:60%;" />



OS/360 是操作系统中一个殿堂级别的操作系统，很多概念都是从这个操作系统中来的，实际上很多事情就是从这故事来的。

如果一台计算机要干很多事，但是仍然采用批处理方式，这个就不太合适了。比如假设 JOB1 是 I/O 的任务，JOB2 是一个计算的任务，如果采用批处理方式的话，那么必须要把 JOB1 完成以后才能去处理 JOB2 的任务。因为若是在 JOB1 还未完成时就切换到 JOB2 ，那之后就再也回不来去处理 JOB1 的任务。

如果在处理 JOB1 的 I/O 任务时，计算机需要等 5 秒去读写磁带，那么对于当时来说这么重要的机器，你让它等 5 秒钟，显然是巨大的浪费。因此，一个自然而然的想法就是，一旦计算机在处理 JOB1 停下来以后，我们可以切换到另外一个任务去执行，等到 JOB1 的 I/O 处理完以后，再切换回 JOB1 继续处理。

多道程序（multiprogramming）：**多个任务同时出现在计算机中，然后多道程序交替地向前推进，这样的话就出现了作业之间的切换和调度，这个时候就出现了操作系统的样子**。什么时候切换？什么时候调度？怎么切换？这就出现了很多事情，这个样子就构成了现代计算机的基础。

为了完成多道程序的切换，让它合理有序地切换和调度，操作系统就要做很多事情。为了做到这些事情后来就出现了 IBM OS/360，当时也才把这个系统叫做操作系统。

什么是操作系统呢？前面我们说过，是给计算机的硬件装了什么系统来方便用户对计算机的使用。当 IBM OS/360 这个系统装在计算机上，就可以用在各行各业。但由于处理任务时切来切去、调度来调度去，最后这个系统变得越来越复杂。它实际上是一个不成功的项目，因为到最后它的 bug 非常多。**但是，它的基本思路是非常对的，多个任务同时存在系统，多个任务之间不断地来回切换、调度**。

**20 世纪 6、70 年代还没有特别好的软件工程思想，所以当时在代码特别多的时候，最后没办法控制了**。当时开发 OS/360 的工程师们，最后画出这样一张图：**操作系统放在中间，而我们这些开发者已经变得像恐龙一样，完全不懂它在干些啥了。所以当时有句话是 “面对如此复杂的操作系统，我们犹如史前动物一样”**。

因此，从这个侧面也可以看出，为了满足计算机应用于各个行业的需求，多进程结构必然会出来。但是多进程结构也会导致系统非常复杂，这个复杂的系统在当时大家对操作系统的认识还比较模糊的情况下，做起来是非常费劲的，而且没有成功。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L6-OS_360-Multiprogramming.JPG" alt="L6-OS_360-Multiprogramming" style="zoom:60%;" />



MULTICS 和 OS/360 对比的话，它在本质上并没有多大的推进，它只是增加了一个内容。在这个系统上除了出现多个任务，还出现了每个人都控制一个任务的这种局面，因此它引入了分时的概念。分时系统对 PC 机来说看起来没什么影响（但 PC 机里也有分时的思想在里面），但对服务器来说非常有意义。

MULTICS 也使得多进程的样子、多任务同时推进的样子更加清晰了。不过核心仍然是任务切换，仍然是多进程结构。虽然操作系统的历史是发展的，但它的基础仍然是多进程结构。**实际上多进程结构是操作系统最核心部分，到现在仍然是这样。所以对当时提出来的 OS/360，我们把它称为殿堂级别的一个系统**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L6-MULTICS-TimeSharing.JPG" alt="L6-MULTICS-TimeSharing" style="zoom:60%;" />



OS/360 和 MULTICS 都没有获得成功，但 UNIX 获得成功了。

Ken Thompson 和 Dennis Ritchi 是图灵奖获得者，他们俩是唯一的因做出系统而获得图灵奖，大神级别人物，非常著名。

实际上 MULTICS 是在 OS/360 基础上改造过来的。

UNIX 几乎是学计算机的必须要知道的一个系统，非常著名。学习 Ken Thompson 和 Dennis Ritchi 的故事也引出了我们这个课程的一个主旨：大家可以看到，找一个自己的小伙伴，两个人配合在一起，当发现这个世界上有感兴趣的东西后，是否可以做出来，用代码实现它。

计算机就是这样的一个行业，大家找那么三、两个人作为伙伴，去写一些实际的东西出来，将来可能会取得辉煌的成绩。像比尔 · 盖茨的微软是这样，谷歌也是这样，我们这里不再说了。当然，要做出这些事情，你必须能有这个能力去写出来。这也是我们这个课程的原因，让大家真正地去接触操作系统，真正具有写出系统的能力。这样的话当你面对一个新的机会的时候，你才有能力去做一些你感兴趣的事。

所以主题就是，找一个伙伴，或两个伙伴，选一个感兴趣的东西，你们有足够强大的编程能力和计算机系统能力，做一点事情。这也是我们学习操作系统历史要给大家传达的一个思想。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L6-UNIX-SimplifiedMULTICS.JPG" alt="L6-UNIX-SimplifiedMULTICS" style="zoom:60%;" />



Linus 在做出 Linux 后，他以开源的形式把 Linux 发在互联网上，因此就展开了一场轰轰烈烈的 Linux 产业化运动。现在 2015 年，自从 2010 后 Linux 非常辉煌，很多系统，像安卓手机，底层就基于 Linux，很多嵌入式设备上面装的也全是 Linux。

**既然 Linux 是改装 UNIX，所以它的核心思想仍然是 OS/360 的多进程结构**。这里也更加印证了一点，一个人做操作系统比明白操作系统背后的原理要更加重要。当然，不明白原理是不行的，**明白纸上的原理对操作系统的学习来说，可以说只完成了 10% ，剩下的 90% 是要真正把它落实到实际的计算机中，因为操作系统就是一个实实在在的软件**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L6-Linux.JPG" alt="L6-Linux" style="zoom:60%;" />



在操作系统的发展历史中，多进程的结构一直没有再改过。

这里是老师给大家说的一句话，根据这句话大家也可以看到我们这个课程的主旨，这也是我认为操作系统就应该这么学：**我们要掌握操作系统的多进程图谱**，如果头脑中没有这个图谱，没有这个样子，那你肯定做不出什么 UNIX，Linux。

但是如果你只有这个图谱，你画在纸上玩一玩还可以，不过没有任何用途。**我们如果能把这个图谱实现，至少我们能把这个图谱理解到别人是怎么实现这个图谱的程度，我们能够适当改造这个图谱，那么你对计算机，你对操作系统的理解就上了一个层次**。

有了这样的一个层次以后，才能兑现我们之前的承诺，我们要通过上层接口进入系统，并且扩充系统，来提高系统的能力。如果你不明白操作系统的多进程图谱，那肯定是无本之末，但如果你要是只知道这个图谱而不去实现它，你就永远不会有这个系统能力。

**所以我们既要在头脑中立起这个图谱来，这个是我们讲 CPU 管理那一部分中，最核心的一项任务，把多进程图谱立起来。此外，不仅要立起来，还要讲它是如何一步步做出来的**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L6-IBSYS-OS_360-MULTICS-Unix-Linux.JPG" alt="L6-IBSYS-OS_360-MULTICS-Unix-Linux" style="zoom:60%;" />



**刚才我们讲了一个图谱，即多进程结构这个图谱，那么接下来我们要看第二个图谱。在我们操作系统的课程上，把这两个图谱说明白了，讲明白了，怎么做出来的讲清楚了，那么我们把操作系统这架钢琴就分解了**。在这里我用第二条历史来解释第二条图谱。

很多人接触 PC 后，就有那些人，一些感兴趣的有才能的人、又能刻苦专研的人、锲而不舍的人，就会对系统、对这个 PC 机做很多事情，其中 DOS 就是著名的代表。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L6-CP_M-QDOS.JPG" alt="L6-CP_M-QDOS" style="zoom:60%;" />



Paul Allen 和 Bill Gates 在杂志上看到了 ALTAIR 8800 这台机器的报道，很感兴趣，想着能不能为这个机器做点什么，于是两个人就开发了 BASIC 解释器。这个非常好，你在上面开发了 BASIC 解释器，那别人就可以用你的 BASIC 解释器来编 BASIC 程序，那么别人就可以用这个机器了，据此开创了微软。

MS-DOS（比尔 · 盖茨的第一个系统） 和 IBM PC 一起打包卖的时候，当时就非常火。老师在那个年代，九几年到二千年的时候，老师 1995 年读的本科，在 2000 左右的时候，用的机器上面装的都是 DOS，当时非常火。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L6-MS-DOS.JPG" alt="L6-MS-DOS" style="zoom:60%;" />



EDIT：每讲到这个地方老师就心绪万千，我第一次接触计算机就是接触这个 EDIT（我本科是学机械的，就是因为这个（EDIT），机缘巧合，最后爱上了计算机，然后再改行做计算机这么多年，我个人觉得计算机还是非常有意思的）。

**从 MS-DOS 到 Windows 打的这张牌，并不打核心的多任务结构，它打的是用户怎么使用得更方便的牌**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L6-Windows.JPG" alt="L6-Windows" style="zoom:60%;" />



Mac OS 不光专注于界面、文件，它还关注媒体、手势这些比较高级的东西。大家知道，可能未来通过手势识别、语音输入、姿势识别、脑电波识别等这些人机交互来操作计算机会成为一种潮流，**但是，它对应的核心思想仍然是文件**，你眨一下眼睛无非也就是控制摄像机拍出一个文件。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L6-Mac_OS-iOS.JPG" alt="L6-Mac_OS-iOS" style="zoom:60%;" />



**文件这个视图就成为我们要讲解的第二个视图**。仍然是这样，也许文件的概念本身并不复杂，讲讲可能半个小时，一个小时就说清楚了，在纸上画一画也很快，**但是，如何让文件变成实实在在的东西，如何编程实现，如何实现它，这个就很重要**。

我们在整个操作系统里面，打开操作系统要做到两件事：

- 第一件事就是要理解多进程图谱，这是个什么东西？它是怎么做出来的？它的原理来自于 OS/360，最后来自于 UNIX。然后是怎么做出来的？我们主要讲 Linux 中的一些代码，来看看它是怎么做出来的。我们要掌握、实现这个多进程图谱。
- 在学完多进程图谱以后再学下一部分，即文件操作视图。我们来看看文件是什么东西？它是个什么概念？实际上 UNIX 里面有这个概念，DOS，Windows 也有这个概念。我们来看看文件这个视图是什么样，这个视图是什么结构，它是怎么做出来的。

&nbsp;

所以在我们的课程里有两大基本的主题，**第一个就是多进程图谱是什么样子，它将会覆盖两个部分（CPU 和内存）；而第二个文件视图对应着 I/O 设备、磁盘**。

**什么是操作系统，要管理计算机的硬件。要管理哪些硬件？怎么管理 CPU？怎么管理内存？怎么管理 I/O 设备？怎么管理磁盘？怎么管理文件？正好对应了那 5 个模块。所以这两个视图，就是操作系统的核心视图**。

如果把这两个视图弄明白了，操作系统就在你的脑中形成了。只有在你的脑中形成了，你才能写出操作系统。你要是能写出操作系统，那么什么样的计算机系统你都可以写得出来，因为操作系统是整个计算机系统中最灵活的东西。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L6-Windows-Mac_OS-iOS.JPG" alt="L6-Windows-Mac_OS-iOS" style="zoom:60%;" />



&nbsp;

## L7 我们的任务

我们这个课程就是要打开操作系统，看看操作系统到底是怎么做出来的。那么操作系统是怎么编写出来的？怎么做出来的？肯定要围绕上一节提到的两个视图，即多进程图谱和文件操作视图展开来讲解。本来直接就开始讲多进程图像，但这里我们将任务再分解一下，让大家看得更加清晰一些。



那么我们采用的方法是什么呢？温故而知新，我们现在将前面所学的内容串一下，串在一起来看一看我们在接下来的内容中，会一段一段地去讲解什么内容。

**操作系统管理的硬件主要有 4 个部分：CPU 、内存、I/O 设备（显示器、键盘、打印机等）、磁盘和磁盘上的文件**。

我们要知道操作系统是如何管理硬件的，它是怎么做到的。怎么个做法呢？我们是通过上层应用进入操作系统的方式，我们想看看上层应用是怎么用操作系统的？到了操作系统里面它是变成什么样的？具体怎么做的？

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L7-OSManageHardware-review.JPG" alt="L7-OSManageHardware-review" style="zoom:60%;" />



这就引出了接口、启动、初始化。

操作系统要管理硬件，怎么管理硬件呢？肯定要设置一些数据结构，根据数据结构来管理。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L7-InitializeSomeDataStructuretoManageHardware-Runshell.JPG" alt="L7-InitializeSomeDataStructuretoManageHardware-Runshell" style="zoom:60%;" />



**这四个部分的管理，实际上已经管理好了单机、单 CPU 上的硬件情况**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L7-ManageFourTypesHardware.JPG" alt="L7-ManageFourTypesHardware" style="zoom:60%;" />



再具体一点呢，我们要通过接口进入到系统。

fork 是什么？为什么它会管理 CPU？fork 出来的东西是什么？凭什么它就可以管理 CPU ？

内存方面：通过 `*p = 7` 操作一个地址，我们就能使用内存，为什么能使用内存？它是怎么做到的？

为什么 CPU 和内存合在一起就能形成多进程图谱？为什么它们可以合在一起？

设备驱动，为什么说它是文件视图？因为设备对应的是操作设备的文件，**操作设备就也是操作文件**，比如操作 dev/tty0 这个文件就可以操作显示器、操作键盘。而操作文件可以用 open，open 一个普通文件发生了什么事？open 一个设备文件又发生了什么事？这是我们后面要讲的核心内容。

所以从下次课开始，我们就要讲多进程图谱中进程是怎么回事？怎么做出来的？在讲这个时会延伸出很多问题。在讲清楚这个以后，我们就开始讲内存是什么概念？为什么 `*p = 7` 可以访问内存？以及 `*p = 7` 访问内存和进程是个什么样的关系？

讲完这个以后，就可以开始讲文件视图，我们要讲清楚一个设备是怎么驱动的？open 一个设备文件是什么？磁盘是个什么东西？open 一个磁盘文件又是什么含义？它们又怎么纳入在统一的文件视图下？

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L7-fork-asteriskP-open.JPG" alt="L7-fork-asteriskP-open" style="zoom:60%;" />



<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L7-cpu+memery+file_management.JPG" alt="L7-cpu+memery+file_management" style="zoom:60%;" />



<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L7-TwoKindsofOSCoreView.JPG" alt="L7-TwoKindsofOSCoreView" style="zoom:60%;" />



&nbsp;

## 附录

1. 实验部分可参考：[实验楼中配套的实验](https://www.shiyanlou.com/courses/115)；[hoverwinter/HIT-OSLab](https://github.com/hoverwinter/HIT-OSLab) 。
2. 课程的辅助教材：《Linux内核完全剖析》0.11 版（[豆瓣链接](https://book.douban.com/subject/1481173/) ；[oldlinux.org-clk011c-3.0.pdf](http://www.oldlinux.org/)）



&nbsp;

&nbsp;

> **版权声明：本文采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh) 许可协议，若转载请注明出处**。