---
title: '哈工大李治军老师讲操作系统 --（三）内存管理'
category: Operating System
tags: 
  - 'OS-Lizhijun'
---

这份笔记，是我看哈尔滨工业大学李治军老师讲《操作系统》这门课所记的第三部分，内存管理。关于这份笔记的说明，可查看前面一篇 [（一）操作系统基础](https://haokaimo.github.io/2020/07/27/OperatingSystem-Lizhijun-PartOne/) 的前言部分。

<!-- more -->



&nbsp;

## L20 内存使用与分段

前面我们讲过，操作系统是管理计算机硬件的一层软件。前面我们主要讲了操作系统如何管理 CPU，那么从现在开始，我们讲操作系统如何管理内存，这是非常重要的一个部分。

讲解的方法和前面实际上是类似的，我们前面讲操作系统是如何管理 CPU 呢？首先讲 CPU 是怎么能用起来的，然后从用起来的直观想法，引出问题，一步步解决这些问题，从而引出了多进程图像、多进程图像的各种支持方法、多进程图像的切换等等。

从这节课开始，我们首先也是讲内存是如何用起来的。所以今天我们讲的是内存如何使用，然后接下来再延伸，在使用的过程中，如何让内存更好地使用，引出了分段的概念。

所以讲解的方法和前面是一样的，先从直观的想法开始，然后一点点引出问题、解决问题，使得操作系统对内存的管理越来越成熟、越来越有效。



如何让内存用起来？仍然从计算机如何工作开始。

如何让内存使用起来呢？很简单，就是将程序放入内存中。所以将程序能放入内存中，并且能够执行起来这个程序，那么内存不就跟着能够一起运行起来、工作起来了吗？

**所以让内存使用的方法非常直观，能将程序放入内存中，然后能让程序执行起来就可以了**。因此接下来的核心就是，如何让程序放入内存中，如何让程序执行起来。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L20-FromComputerHowToStartWorking.JPG" alt="L20-FromComputerHowToStartWorking" style="zoom:60%;" />



实际中应该是这个样子，应该在内存中找一段空闲的单元、找一段空闲的内存，然后将这段程序放进来。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L20-LoadProgramIntoMemoryFirstly.JPG" alt="L20-LoadProgramIntoMemoryFirstly" style="zoom:60%;" />



同时也要修改 `call 40` 中的地址 40。**程序中的地址 40 是相对地址，但现在放到物理内存中了，就要变成物理内存的一个物理地址。我们通常把这个相对地址，在后面也叫做逻辑地址，所以这个 40 是逻辑地址**。

我们需要修改程序中的地址，**这种修改有个名字叫重定位**。因此，总的来说就是我们需要找一段空闲的内存，然后将程序载入到这段内存。在放入到这段内存中后，需要进行重定位，在重定位后，要设置好 PC（程序的初始地址），然后取指、执行，取指、执行。当程序顺利地执行起来了，内存也就被顺利地使用了。

什么时候完成重定位？编译时（嵌入式系统、卫星上等静态系统可以使用这种方式）；载入时（更灵活）。

在载入时重定位的程序，一旦载入内存就不能动了，这样好不好呢？也不好，很多时候程序载入以后还要移动。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L20-Relocation-When.JPG" alt="L20-Relocation-When" style="zoom:60%;" />



程序载入后还需要移动。

一个重要概念（swap）

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L20-ProgramLocationInMemoryStillNeedsChanging.JPG" alt="L20-ProgramLocationInMemoryStillNeedsChanging" style="zoom:60%;" />



**所以，应该是运行的时候重定位。运行的时候重定位也叫地址翻译**。

基地址（base）放在进程的 PCB 里。

我们整理一下。怎么让内存使用起来呢？首先将一个程序编译好，这时候程序里面的地址不需要做任何修改。然后接下来让它执行，要让它执行需要干什么事呢？要想让程序执行起来，需要创建进程、需要创建 PCB。要做的工作是什么呢？**就是在内存中找一段空闲的内存，然后把这段空闲的内存起始地址赋给 PCB。接着将 PC 置为这段程序的初始地址，然后就开始执行。每次执行程序时，都要进行地址翻译**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L20-TheBestTimeToRelocation-WhenRunningTheProgram.JPG" alt="L20-TheBestTimeToRelocation-WhenRunningTheProgram" style="zoom:60%;" />



你要是把这幅图明白了，内存的使用、多进程图像、进程连带着内存的使用，这些图就全明白了。这些图明白了，实际上操作系统的内核，就一点点出来了。这就是操作系统对内存的使用。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L20-OrganizeTheThoughtsAboutUsingMemory.JPG" alt="L20-OrganizeTheThoughtsAboutUsingMemory" style="zoom:60%;" />



上面讲的是内存使用起来的最直观的想法、基本想法，那么接下来我们要一点点向前推。第一个向前延伸的就是分段。**之前讲的是将整段程序放入内存中，但实际情况不是这样的，实际上程序由若干段组成**。

采用分治的方法，来处理程序中不同的段。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L20-DoesItLoadsTheWholeProgramIntoMemory.JPG" alt="L20-DoesItLoadsTheWholeProgramIntoMemory" style="zoom:60%;" />



<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L20-ProgramInProgrammer'sEye.JPG" alt="L20-ProgramInProgrammer'sEye" style="zoom:60%;" />



这个时候一个程序分成好多个段了，每个段就可以分别放入内存，而不是将整个程序一起放入内存。这种方法也让内存的使用效率提升。**一旦将程序一段一段地放入内存后，再寻址时就要用段号加上偏移（<段号，段内偏移>）。这也就是说，在 PCB 里现在需要放每个段的基址，不再是之前那样只放一个基址**。

进程段表。我们前面有讲过 GDT，根据 CS 去查 GDT，著名的那个 `jmpi 0,8` ，就是根据 8 这个 CS 找 GDT，找出那个基址再和 0 相加。所以 GDT 这个概念不就有了吗？前面一直不知道 GDT 是怎么出来的，是什么东西，今天我们终于可以回答在讲操作系统启动时，讲过的这个东西。

实际上这个表就是 GDT 的样子，但不是真的 GDT。因为 GDT 处理的是 OS，我们把运行的操作系统程序假装也看作是一个进程，操作系统那个进程对应的段表就是 GDT。每个进程都有一套段表，操作系统那套段表叫 GDT，而普通进程的段表叫 LDT。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L20-LoadDifferentSegmentsOfProgramIntoMemory.JPG" alt="L20-LoadDifferentSegmentsOfProgramIntoMemory" style="zoom:60%;" />



这个表似曾相识...，真正故事：GDT+LDT

在段表的这个基础上如何使用内存呢？把程序分成一段一段，每个段在内存中找到一个空闲的地方，把这段空闲内存的基址放在一个 LDT 里。当一个程序载入到内存中后，把初始化好的 LDT 赋给这个进程的 PCB ，接着把 PC 设置好初值，之后就开始取指、执行，取指、执行。

在每执行一条指令时，都查 LDT，找到程序中的那些地址，然后根据表中的基址和程序中的地址，就可以找到相应的物理内存地址，这就是内存使用的第一个部分。现在大家可以看到，内存就可以完全使用起来了。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L20-GDT+LDT.JPG" alt="L20-GDT+LDT" style="zoom:60%;" />



&nbsp;

## L21 内存分区与分页

操作系统管理 CPU，也要管理内存，这两部分管理好了，一个基本的操作系统就有了。再加上后面要讲的设备驱动和文件系统，比如管理磁盘，那么一个比较完整的、可以使用的小操作系统就出现了。

今天要继续往下讲内存管理的内容，我们先回顾下上次课讲的内容。回顾的目的有两个：第一个，是希望加深一下认识；第二个，回顾的内容要引出今天的这个问题。

实际上上次讲的内容不能成为完整的体系，还差一点内容。差的内容就是今天要讲的第一部分，内存分区 。内存分区有承上启下的作用，这个分区会引出一个问题。而求解这个问题，就会引出另一个操作系统管理内存的主题，分页。

**前面我们讲的是分段，而今天要引出的内容是分页。实际上最后大家可以看到，段和页合在一起，才是真正的操作系统内存管理**。

我们接下来就开始一部分、一部分讲。第一部分我们简单回顾一下前面的内容，引出内存分区；然后第二部分我们讲解内存分区，引出分页；第三部分讲解分页，这一讲就完事。那么大家就会对页有了深刻的认识，再和前面的段合在一起，就看到了操作系统对内存管理的一个基本的轮廓。

寄存器 ldtr 指向 LDT 的起始地址，LDT 用硬件实现。

关于内存使用时，LDT、运行时重定位、地址翻译等这部分内容，大家要做做实验，要自己在纸上画一画，要自己写个程序去看一看它的地址是怎么变化的、放到哪里，要自己做做实验。

我们的实验六就是和这个内容相关的。大家要回去认认真真做实验六，不做完这个实验，光靠老师在这讲一讲，你们听一听，那还没有深刻地认识。所以自己要做实验，要真实地看一看这些地址是不是按照这种方式变化的，然后将这个图在头脑中逐渐形成。

这是回顾上次课的内容，也是我们这节课讲的第一部分。第二个部分呢，是上次课讲的内容中有一个部分没讲，差哪一块呢？我们将这整个过程总结起来实际上就三步：

- 第一步，将程序分成多个段。实际上这是编译要做的事，不归我们管，那么好，假设程序已经分好了段；
- 第二步，我们要在内存中找一段空闲地方、空闲分区，在内存中找一段空闲的区域，实际上老师这就不经意地引出了这句话，内存分区，这就是今天要讲的第二部分，在内存中如何找出一段空闲的分区；
- 第三步，一旦找出内存中一段空闲的分区来，我们就把程序中的段读入到这里。当然，讲到后面的文件系统、磁盘管理、设备驱动才能知道怎么读。我们假定读进内存了，在读的过程中还要把 LDT 初始化好，并且 LDT 和 PCB 放在一起，这就是整个的三步。

&nbsp;

**第一部分分段，第二部分找一段空闲分区，第三部分要通过一个表将地址的映射关系做好，将这个映射表做好。这样的话程序在执行时，通过运行时重定位就可以取指、执行，取指、执行。这样的话程序就执行起来了，内存也就跟着用起来了**。

由此可以看到，这个故事虽然凝练成这三步，但实际上它涵盖了计算机很多方面，有编译，有设备驱动，有算法、数据结构，有内存管理，有进程管理，有表的填写等。所以大家可以看到，这就是操作系统，越讲到后面它就越会糅合很多内容在里面。

所以它是一个系统嘛，System，**讲系统的时候，大家就是要既能够深入到各个具体的细节，又能够把它凝练成一些基本的步骤**，这就非常重要。这是一句题外话。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L21-MemoryPartitionAndPaging.JPG" alt="L21-MemoryPartitionAndPaging" style="zoom:60%;" />



接下来我们开始讲实际的内容，讲内存分区。如何在内存中找出一段空闲的区域？也就是说内存怎么割？如果一旦知道内存该怎么割了，那么就可以将程序的各个段载入到相应的内存分区中。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L21-HowToSegmentMemory.JPG" alt="L21-HowToSegmentMemory" style="zoom:60%;" />



我们首先讲一种最简单的方法，固定分区。

固定分区与可变分区。实际采用的是可变分区的思想。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L21-FixedPartitionAndVariablePartition.JPG" alt="L21-FixedPartitionAndVariablePartition" style="zoom:60%;" />



可变分区的管理过程——核心数据结构

**计算机中非常常见、非常基本的一种想法就是通过一些数据结构，来记录信息，并且更新信息，通过信息来管理客观事物**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L21-VariablePartitionManagement-CoreDataStructure.JPG" alt="L21-VariablePartitionManagement-CoreDataStructure" style="zoom:60%;" />



可变分区的管理——请求分配

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L21-VariablePartitionManagement-RequestForAssignment.JPG" alt="L21-VariablePartitionManagement-RequestForAssignment" style="zoom:60%;" />



可变分区的管理——释放内存

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L21-VariablePartitionManagement-ReleaseMemory.JPG" alt="L21-VariablePartitionManagement-ReleaseMemory" style="zoom:60%;" />



可变分区的管理——再次申请

讲完这个地方，第二部分就讲完了，多简单啊，整理一下。也就是说，操作系统为了让内存能够使用起来，将程序打成多个段，每个段要放在内存的不同地方，当然，要放在内存空闲的地方。所以，就是要在内存中找到哪些地方是空闲的。

为了找到哪些地方是空闲的，需要做两件事：第一件事是要维护这个数据结构（空闲分区表）；第二件事，要给一个算法，当内存段的请求来了以后，选哪个空闲的内存段去分配呢？

所以要选一个算法和这个数据结构搭配起来。比如说就选首先适配算法，用这个算法，写一段程序，加上这个数据结构，再加上刚才讲的前面我们回顾的段的结构，再加上 PCB，修改 LDT 那些代码（关于 PCB 初始化的代码，在前面我们实际上看过，在那里面添加进去）。

大家可以看到，一旦把这些合在一起，内存就管理起来了。并且 CPU 也可以通过这种内存管理的方法，来使用内存。一旦能使用内存，就能取指、执行，实际上一个最小的操作系统就可以有了。

我们讲的不仅是最小的操作系统，而是一个基本的操作系统，是一个基本的现代操作系统。那么不仅仅支持分段，还支持分页。所以接下来下一个话题，我们要讲为什么会有分页，以及怎么来进行分页。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L21-VariablePartitionManagement-RequestForAssignmentAgain.JPG" alt="L21-VariablePartitionManagement-RequestForAssignmentAgain" style="zoom:60%;" />



根据你的操作系统对内存使用的特点，来决定使用哪种算法。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L21-AQuestionAboutChoosingAMemoryPartitionAlgorithm.JPG" alt="L21-AQuestionAboutChoosingAMemoryPartitionAlgorithm" style="zoom:60%;" />



在实际操作系统中，并不使用内存分区的东西来进行最后物理内存的分割，**实际上是通过分页来做的**。

为了让段放到内存中，需要割一段区域，而物理内存是用分页来割的。**实际上中间还有个东西，这就是后面要讲的虚拟内存**。这里只是引下，埋个伏笔。

**分段，段对应虚拟内存，虚拟内存最后要落实到实际的物理内存，而物理内存的分割方法采用分页的方法**。这样,段、页内存就合在一起，这是下次课的核心话题——如何将段页合在一起。

那么接下来我们就开始引出分页，这是这一讲的最后一个话题。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L21-IntroduceMemoryPagingToIncreaseEfficiency.JPG" alt="L21-IntroduceMemoryPagingToIncreaseEfficiency" style="zoom:60%;" />



可变分区造成的问题：内存碎片

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L21-ProblemsCausedByVarialblePartition.JPG" alt="L21-ProblemsCausedByVarialblePartition" style="zoom:60%;" />



从连续到离散

**操作系统一初始化的时候，物理内存就分成一页一页的。分页的好处：不用进行内存紧缩；内存的浪费非常少，比如一个段最多浪费一页（4k）**。

从物理内存的角度，希望采用分页，这样浪费少；而从用户的角度呢，用户希望采用分段，因为程序是由多个段组成的。所以物理内存希望分页，用户希望分段，因此这两个特点应该都支持。

既支持分段又支持分页，所以这也就是我们所说的，操作系统既支持段又支持页，段和页合在一起，这是下节课的主题。

这样的话内存管理的轮廓就应该有了。**第一讲我们讲了分段，这一讲我们核心是分页，下一讲我们要讲的是段、页合在一起，一个基本的轮廓就有了**。再加上后面一点内容，这个操作系统的内存管理，基本上就建立起来了。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L21-FromContinuousToDiscrete-Paging.JPG" alt="L21-FromContinuousToDiscrete-Paging" style="zoom:60%;" />



存放页表指针的寄存器为 cr3，每个进程有自己的页表，进程切换的时候 cr3 跟着切换。当 cr3 切换后，它就指向当前的页表。程序在执行的过程中，比如遇到逻辑地址 40，那么就要根据这个逻辑地址去页表中查，算出它是哪一页，然后找出它对应的页框号，再进行补齐（offset）。

用逻辑地址去除以页面尺寸大小（比如 4k），得出的除数和余数，分别对应页号和偏移数，在得出页号后根据页表查出对应的页框号。这些地址重定位的计算，是由 MMU（内存管理单元）这个硬件做的，每个 CPU 上都有这个东西。只要把页表的起始地址（指针）给它，当进行逻辑地址转换时，分页的那套电路启动，就会得出相应的值。

也就是说，只要把这个页表交给 PCB，PCB 切换的时候 cr3 跟着切换，然后程序执行遇到重定位时，MMU 再自动去运算得出相应的值， 这就是页的工作。

讲到这个地方大家可以看到，一个程序中的段就要打散成许多页，这些页就放在内存中的页框中，然后建立这个页表，并且和 PCB 关联起来。这和前面的段表非常相似，这样在进行地址重定位时，首先去查页表，根据页表的内容算出它的物理地址。

关于页的故事就结束了，这个时候就变成这个样子：一个程序是由多个段组成，每个段不是直接放在内存，如果直接放在内存会产生内存碎片、内存浪费的问题。因此一个段就要打散成多个页，每个段就要放在内存中的多个页。为了将来能够实现重定位，能够找到程序执行时的地址，就要建立页表，然后这个页表和 PCB 要关联，这就是关于分页的故事。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L21-Redirection-PagingTable.JPG" alt="L21-Redirection-PagingTable" style="zoom:60%;" />



&nbsp;

##  L22 多级页表与快表

为什么要有这一讲呢？实际上主题词就是页。在上一讲的时候，我们讲过了分页机制。也就是说，操作系统将物理内存分成固定大小的页，然后根据页来分配内存。这样的优点就是空间利用率高，没有内存碎片。**但是，单独的分页机制会出现问题，这个问题就需要借助今天讲的这个多级页表和快表来解决**。

实际上就是说，前面讲的那个分页机制，在纸上看起来没有问题，但是在实际系统中使用时会出现问题，这个问题需要用今天要讲的多级页表和快表来进行解决。

也就是说，多级页表与快表和前面的分页机制合在一起，就形成了比较完善的、在实际中可以比较高效工作的一套完整机制。所以，这就是这堂课的核心内容，也就是说，要解决分页出现的问题。



那么分页会出现什么问题呢？我们的故事就要从这里开始。分页会出来什么问题呢？**核心就是页表的问题，当页小了时，页表就大了**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L22-TheSizeOfPageIsSmaller,ThePageTableIsBigger.JPG" alt="L22-TheSizeOfPageIsSmaller,ThePageTableIsBigger" style="zoom:60%;" />



当页表大了，页表的放置就成了问题

实际上一个进程中大部分逻辑地址根本不会用到。写的程序中不是所有的逻辑地址（比如 [0, 4G] ）全使用，可能是使用 [3G, 3G+4M] 的这一段空间。

在这个 PPT 的例子中，页号 2 的这一页，逻辑地址是没有使用的。那么既然逻辑地址没有使用页号 2，不需要对应物理地址，可不可以将这个页表项删掉。如果可以删去一些这样的页表项，那么，这个页表的大小在原来的基础上也就减少了，这样的话就避免了因为存储页表带来的内存占用。

页表本身不是用户的程序，是操作系统的一种负载、一种损耗。那么这种损耗可不可以减小？所以，这是一个非常直观的想法。能不能把不使用的逻辑页号从页表中去掉？因为 32 位地址， 4G 的内存空间不是每一个都使用。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L22-WhenPageTableIsBig,It'sStorageIsAProblem.JPG" alt="L22-WhenPageTableIsBig,It'sStorageIsAProblem" style="zoom:60%;" />



第一种尝试，只存放用到的页

第一种尝试失败了。只存放用到的页不行，用不到的页表项也要保留，这样使得页号连续。那么这样就会造成页表很大，占用大量的空间，造成浪费。

既要页号连续，又要让页表占用内存少，怎么办？**我相信，在面对这个问题的时候，设计操作系统的人肯定想了很久，最后人们提出了一种方案——多级页表，用多级页表既实现连续，又让页表占用的内存变少**。

用书的章目录和节目录来类比思考。页表仍然是连续的，第一章、第二章、第三章、第四章、第五章，而第五章中第一节、第二节、第三节、第四节、第五节等还是连续的。当要查找的页号在第五章时，就不用查看前 40 节（假设每章有 10 节）。

也就是说，这前 40 节的内容不需要放到内存中（不需要看就是不用比对、不用查嘛，所以这一部分不需要放到内存中）。那么，这就使得页表高效利用空间，少占用物理内存，而且仍然保持连续。这就是它的思想，借鉴了现实生活中章节的概念而引出了第二种尝试——多级页表。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L22-TheFirstIdea-JustStorePagesWhichWillBeUsedInThePageTable.JPG" alt="L22-TheFirstIdea-JustStorePagesWhichWillBeUsedInThePageTable" style="zoom:60%;" />



多级页表。既是连续的，又保证放到内存中的页表项要少了很多。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L22-TheSecondIdea-Multi-levelPageTable.jpg" alt="L22-TheSecondIdea-Multi-levelPageTable" style="zoom:60%;" />



多级页表对空间的利用率比较高，但在时间上也引出了额外的负载。为了解决这个问题引出了 TLB。**TLB 是一组相联快速存储，是寄存器。它可以非常快地找到你最近使用的逻辑页对应的物理页号**。

最终的结果是快表+多级页表合在一起。形成了这种结构，既保证了时间上的快，而且空间利用率也比较好。这也充分体现了操作系统的一种折中的思想。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L22-Multi-levelPageTableIncreaseAccessingMemoryTimes-IntroduceTLB.JPG" alt="L22-Multi-levelPageTableIncreaseAccessingMemoryTimes-IntroduceTLB" style="zoom:60%;" />



TLB 得以发挥作用的原因

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L22-TheCauseOfTLB'sEfficiency.JPG" alt="L22-TheCauseOfTLB'sEfficiency" style="zoom:60%;" />



为什么 TLB 条目数可以在 64-1024 之间？利用了程序的地址访问存在局部性。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L22-WhyTheNumberOfTermsInTLBCanBe64To1024-LocalityInSpace.JPG" alt="L22-WhyTheNumberOfTermsInTLBCanBe64To1024-LocalityInSpace" style="zoom:60%;" />



&nbsp;

##  L23 段页结合的实际内存管理

这一讲，我们讲一个真正的内存管理。那么也就是说，是将段和页结合在一起的内存管理。



程序员希望用段，物理内存希望用页。而作为一个操作系统，既要向上（用户）负责，也要向下（管理物理设备）负责。所以操作系统必然要有这两个部分（段与页），这两种机制要结合在一起。

那么，怎么结合？这是我们要讲的第一个主题。讲完第一个主题，我们把段、页结合的轮廓抽取出来，然后我们开始讲第二个主题。也就是说，这个段、页结合的轮廓是如何具体实现的？这就是今天这一讲要讲的内容。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L23-ProgrammerWantsSegment-PhysicalMemoryWantsPage.JPG" alt="L23-ProgrammerWantsSegment-PhysicalMemoryWantsPage" style="zoom:60%;" />



首先我们说一下段、页如何结合，它是一个什么样的想法。回顾下前面所学的内容，温故而知新，大家温习一下，看看能不能想出来办法，把段和页结合在一起。

地址空间；虚拟内存。

我们从应用程序的角度出发的话，我们需要找出一段地址，本来应该从内存地址中去割，但是现在不能用内存地址，因为为了支持页。我们现在在虚拟内存上割出一个区域来给这个段，那么现在这个段就有了映射。但是，不能真正地往虚拟内存里放东西，那么需要把虚拟内存再映射一次，把虚拟内存的段分割成固定大小的页，然后每一页再去映射到物理内存上。在完成这个映射后，虚拟内存就和物理内存联系起来了。

**操作系统用程序实现了虚拟内存的概念，正好就把段、页这两个概念接在一起**。也就是说，从用户的角度来看，用户的程序对应到虚拟内存上，所以给用户的感觉是有段。从物理内存的角度来说的话，是虚拟内存的段映射到物理内存的页上。所以这个概念非常漂亮，刚好这两个东西合在一起，这就是既支持段，又支持页的核心轮廓，关键在于引入虚拟内存的概念。

这一副样子非常重要。这个样子是理解整个操作系统中，我们这个基本操作系统中，关于段、页式内存的最核心、最关键所在，大家要认真体会这幅图。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L23-CombineSegmentAndPage-SkeletonMap.JPG" alt="L23-CombineSegmentAndPage-SkeletonMap" style="zoom:60%;" />



接下来我们就可以再详细地展开来说下。段、页同时存在，段面向用户，页面向硬件。

对于用户来说，完全是段的使用方式；对于物理内存来说，完全是页的使用方式。所以既支持段，又支持页。有了虚拟内存的这个概念，就可以使得段和页有机地结合在一起，是非常漂亮的一个概念。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L23-SegmentAndPageExistTogether-VirtualMemory.JPG" alt="L23-SegmentAndPageExistTogether-VirtualMemory" style="zoom:60%;" />



段、页同时存在时的重定位（地址翻译）

首先，我们通过段表找出一个地址，找出的这个地址是虚拟地址。虚拟地址需要再经过一次映射，才能得到物理地址，这次映射是支持分页机制，所以要根据虚拟地址算出它的页号，然后根据页表得到物理页框号后，再根据页内偏移，组合在一起形成物理地址。最后操作系统把得到的物理地址发在地址总线上。

以 `jmp 40` 为例，逻辑地址 40 需要经过两层地址翻译，**第一层地址翻译是什么呢？基于段的地址翻译，所以它是支持分段的；第二层地址翻译是基于页的地址翻译，所以它是支持分页的**。因此，从用户的角度看起来完全是用段的方式来使用、来编写程序。在程序段中，地址的连续偏移会映射到虚拟地址上的地址，而虚拟地址再映射到物理内存页框号上的实际地址。

大家要建立起这幅图，在头脑中要有虚拟内存的概念，用这个概念将段和页组合在一起。这是段、页结合这一部分最灵活、最核心性的东西，把它理解了，那么我想段、页同时存在，以及包括后面的代码就非常好理解了，你自己写出这一部分也就很容易了。

好了，那么我们不再重复了，希望大家认真去体会一下这个内容，最好是通过实际的实验，自己动手实践能更加深刻地去体会这个。所以大家回去要做实验六，实验六就是关于段、页内存管理的一个故事。大家要认真去做实验六，实验六首先要做的一个内容就是要跟踪重定位的过程，也就是给你一个逻辑地址，怎么找到虚拟地址，然后又怎么找到物理地址。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L23-SegmentAndPageExistTogether-Redirection.JPG" alt="L23-SegmentAndPageExistTogether-Redirection" style="zoom:60%;" />



**经过翻来覆去地论述，核心就在于虚拟内存，核心就是将段和页合在一起，核心就是从逻辑地址到虚拟地址，从虚拟地址再到物理地址的这两层映射**。

操作系统用这两层映射实现了，既面向用户能支持段，采用段的结构，又面向物理内存采用页的结构。所以是将段、页结合在一起的一个思路，**灵魂就在于虚拟内存**。

好了，第一部分关于段、页内存合在一起的这个概念、轮廓我们就讲完了。接下来我们就要看一个实际的段、页内存是如何做到的，主要来看看代码。实际上看完大家会发现，原来真正要实现段、页内存管理也不困难，核心代码、最骨架型的那些实现语句也就那么几句。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L23-AnActualSegmentAndPageMemoryManagement.JPG" alt="L23-AnActualSegmentAndPageMemoryManagement" style="zoom:60%;" />



好了，那么这个故事从哪里开始呢？从进程 fork 中的内存分配开始。从哪里结束呢？从最后能使用内存，这个程序执行起来能够顺利地让 *p 这个地址单元存储成 7 这个值，那么这个故事就 ok 了。所以回到我们这个主题，操作系统要让用户使用内存。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L23-BeginFromMemoryAllocationDuringProcessFork.JPG" alt="L23-BeginFromMemoryAllocationDuringProcessFork" style="zoom:60%;" />



段、页式内存下程序如何载入内存？

接下来我们就把这个轮廓具体写成代码。整个过程分成几步呢？

- 第 1 步，要在虚拟内存中割一段出来；
- 第 2 步，要把程序段假装已放在内存中，建立好段表；
- 第 3 步，在物理内存中找空闲页；
- 第 4 步，建立页表，再加上磁盘读写就可以把它真正载入到物理内存中。
- 第 5 步，可以用重定位具体地使用内存。

&nbsp;

实际上后面的代码就是讲清楚这 5 部分就完事。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L23-HowToLoadProgramIntoSegmentAndPageMemory.JPG" alt="L23-HowToLoadProgramIntoSegmentAndPageMemory" style="zoom:60%;" />



 故事从 fork() 开始。fork() 要做到 1、2、3、4 这 4 步，运行时 `*p = 7` ，就是第 5 步。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L23-BeginFromFork()-AllocateVirtualMemory-EstablishSgementTable.JPG" alt="L23-BeginFromFork()-AllocateVirtualMemory-EstablishSgementTable" style="zoom:60%;" />



进程 0 、进程 1 、进程 2 的虚拟地址

每个进程占 64 M 虚拟地址空间，互不重叠。

这种虚拟地址的分割方案好不好呢？肯定太弱智了。当然，虽然弱智、虽然简单，但是我们说过，我们首先要明白一个基本的操作系统、简单的操作系统。我们不是一下子去学习特别复杂，比如 Linux 2.6 、Linux 3 点多，不去学习这个。我们明白一个基本的操作系统，那么剩下来的东西，比如改造、变化、扩展、优化等都会有一个基础，我们先学明白基础的。

每个虚拟地址要对应页表，从图中可以看到进程 1 的虚拟地址和进程 2 的虚拟地址互不重叠，那么它们分别对应的页表重叠吗？对应的页表肯定不重叠。把虚拟地址转换为页号来查找物理内存的页框号时，虚拟地址不重叠，物理内存的页框号也不会重叠，所以大家可以共用一套页表。

因为这个进程查这一段虚拟内存对应的页表，那个进程查那一段对应的页表，不打架，不会去查页表中相同的页号。所以，这里的不重叠导致页表可以共用一套，这是一种简化的方法。当然，**在实际的操作系统中，页号是很有可能会重叠的（个人注释：因为不同进程使用的虚拟内存可能重叠），所以每个进程要有自己的页表、自己的段表，大家要注意这个区别**。

为什么要讲这个区别呢？实际上这是一种简化的方法。因为我们要在 Linux 0.11 上做实验，在做这个实验时你可能会觉得和我们讲的内容中，每个进程要自己切换段、切换页，但感觉它没有切换页呢，原因就在这里，不切换是有它的道理的。实际上应该切换，只是因为不重叠，所以切不切换没什么影响，可以不切换。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L23-TheVirtualMemoryAddressOfProcess0_Process1_Process2.JPG" alt="L23-TheVirtualMemoryAddressOfProcess0_Process1_Process2" style="zoom:60%;" />



接下来应该是什么了？第 1 步、第 2 步做好了，割虚拟内存、段表建好了，接着就需要分配内存、建页表。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L23-TheNext-AllocatePhysicalMemory-EstablishPageTable.JPG" alt="L23-TheNext-AllocatePhysicalMemory-EstablishPageTable" style="zoom:60%;" />



<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L23-from_dir-to_dir.JPG" alt="L23-from_dir-to_dir" style="zoom:60%;" />



<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L23-from_page_table-to_page_table.JPG" alt="L23-from_page_table-to_page_table" style="zoom:60%;" />



接下来应该干什么呢，就是把 to_page_table 根据 from_page_table 拷贝过来。

讲到这个部分，第 2 大项工作就完事了，第 1 大项工作是割虚拟内存、建段表，第 2 大项工作是分配物理页、建页表，在分配物理页时，没分配，使用父进程的页。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L23-PageTable-ReadOnly-mem_map.JPG" alt="L23-PageTable-ReadOnly-mem_map" style="zoom:60%;" />



经过这两大项工作最后形成的结果是，进程 2（根据进程 1 fork出来的）的段表有了、虚拟内存有了，对应的物理内存有了、页表有了（完全拷贝父进程的页表），现在父子进程都有了自己的虚拟内存、物理内存，并且虚拟内存和物理内存已经关联好了，段表有了、页表也有了。

现在可以说一个进程、一个程序它就被放到内存中，尽管这个程序用到的代码、数据都是父进程的，但是能够执行的样子有了。当然，要做成自己的，改造一下就可以。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L23-Program+VirtualMemory+PhysicalMemory.JPG" alt="L23-Program+VirtualMemory+PhysicalMemory" style="zoom:60%;" />



接下来我们讲第 5 步。实际上前面几个步骤中最核心的代码，申请虚拟内存、建段表就用了 3 句话。而申请页表的话用 copy_page_tables 先得到页目录，然后再拷贝页表，实际上核心也就那么 3、5 句话。所以最后也就是说，10 句话，核心轮廓就全有了。

我们别忘了，把程序放到内存中，那只是第 1 步，还要用起这个内存。那么要怎么用起来呢？看下 `*p = 7` 是怎么工作的。操作系统一旦把段表、页表做好以后 `*p = 7` 非常简单，实际上我们这里只是验证一下它真能用起来。

根据进程 1 fork 出来的进程 2 在执行 `*p = 8` 时，最开始找的 p 的地址还是父进程 p 的物理内存地址。这个时候要往这个地址处写 8 时，由于这个地址处已被设置成只读的，所以进程 2 写的时候就要进行分离。所谓分离就是新申请一个内存页，修改这个页表，建立一个新的映射，然后在新申请的物理内存页中把 8 写进去。

大家可以看到，父进程 p 的地址用的是值为 7 的这个内存页，而子进程现在用的是值为 8 的这个内存页，大家各干各的事。以后进程访问，父进程访问 p 的地址时，在值为 7 的这个内存页访问，子进程在值为 8 的这个内存页访问。父子进程通过这一套映射表实现了地址的分离，消除了相互的影响。

讲到这个地方，相信我也不用再多说了，大家把这一幅图记在脑子里，那么我们就明白一个程序是如何载入内存的。首先在虚拟内存中割，然后在物理内存中割，建立段表、建立页表，把这些都做好了以后了，在执行的时候 MMU 自动地完成地址翻译。

程序中用到的逻辑地址最后能真的放到物理内存中，并且完成了多个进程在内存中相互影响的消除。当进程切换的时候，LDT 会跟着一起切换。进程拉动内存的这个全部图像，在这幅图中都有了详尽的展现，把这幅图放在脑子里，你渐渐地就明白操作系统是怎么回事。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L23-EliminateInteractionBetweenParentProcessAndChildProcess..JPG" alt="L23-EliminateInteractionBetweenParentProcessAndChildProcess." style="zoom:60%;" />



&nbsp;

## L24 内存换入-请求调页

这一讲讲什么呢？讲内存的换入，下一讲讲内存的换出（swap out）。实际上，这两讲是合在一起称为内存的换入换出。

当然，内存的换入、换出讲完了，操作系统关于内存管理这一部分，我们这课程关于内存管理的这一部分也就结束。讲到这个地方我们就需要知道，为什么要讲内存的换入、换出？实际上学完这两次课后马上可以看到，内存的换入、换出就是从虚拟内存引申出来的。也就是说，为了实现虚拟内存，就应该换入换出。

虚拟内存是实现分段和分页的关键所在，而分段和分页是操作系统管理内存的两个重要的机制。实现内存的换入、换出，是实现虚拟内存所必须的。没有内存的换入、换出，实际上虚拟内存是做不了的。待会儿你马上可以体会到，没有换入、换出，虚拟内存是做不了的。

那么，我们将这整个故事完整地串一下。在这个地方，因为关于内存管理的这一部分马上就结束了。结束以后，整个操作系统对内存的管理，那肯定是一个具体的东西。因为最终落实时，内存管理是操作系统的一个模块。也就是说，我们前面所讲的关于内存管理的内容必须糅合、综合在一起。

那我们在这个地方稍微捋一下，大家也就明白了我们为什么会有这节课，以及操作系统中内存管理的这几讲，实际上是 6 讲，最后会凝结为、综合为一个东西，那就是操作系统内存管理模块。明白了是这样，能把它前后关系建立起来，形成一个宏观的整体，那么大家就明白了操作系统的内存管理。

那么我们来捋一下，我们首先讲的是分段，操作系统为了让用户使用，需要分段。后来我们又讲了分页，因为仅有分段会造成物理内存的空间利用率低，所以我们引出分页。接着我们说，既应该分段又应该分页，所以讲的是分段加分页。而分段和分页合在一起的核心、关键所在就是虚拟内存。而要想实现虚拟内存，就必须要有换入和换出机制。

所以大家可以看到，这不就接到一起了吗？**所以整个操作系统关于内存管理的核心，就是基于虚拟内存的分段加上分页，而用换入、换出来实现这个虚拟内存**。

这一讲我们就要来深入地看一下，实际上非常简单，我们深入地来看一下内存是如何实现 swap in。



我们刚才讲了大半天，这个故事就应该从虚拟内存开始，因为引入换入和换出的根本原因就是虚拟内存。什么样的思想呢？就是那张图，段、页同时存在的时候就要有虚拟内存。应用程序、上层的用户怎么使用内存呢？在他们眼里，他们看不到物理内存，他们看到的就是一个 [0, 4G] 的地址空间（我们讲的都是 32 位的机器）。内存无非就是一堆地址嘛，所以看起来就像是内存有 [0, 4G] 。不管你的实际内存是多少，在用户眼里都是 [0, 4G] 这么多个地址可供他使用。

那么它具体的做法是，将来要使用的时候，首先在这个地址空间中割出一段来作为用户段、割出一段来作为数据段等，怎么割，怎么弄都可以，可随意使用。那么，这不就是虚拟内存的含义吗？接下来还需要对虚拟内存的地址做映射，只有映射到实际物理内存上了，那么程序发出来的指令，发出来的数据才能真正地从物理内存上取出来。

当然，在用户的眼里就是这 4G 的空间。所以从用户的角度，从用户的眼里来看的话，因为后面对用户是透明的，用户看到的就是 [0, 4G] 这段规整的内存，可供他随意使用。操作系统给用户提供了这样一个假象，实际上这个假象就是段，支持用户的段，用户在 4G 空间中可随意划分段、随意使用，给用户形成了这样一种感觉。

当然，这种随意划分段、随意使用，如果不做后面的映射，即从虚拟地址到物理内存地址的映射，那么就用不了。但是，从虚拟地址到物理内存地址是怎么映射的，用户是不知道的。**所以操作系统就是通过映射、巧妙地通过映射，来给用户提供 4G 内存的假象**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L24-SegmenAndPageExistTogether-VirtualMemory.JPG" alt="L24-SegmenAndPageExistTogether-VirtualMemory" style="zoom:60%;" />



<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L24-MemoryInUser'sEye.JPG" alt="L24-MemoryInUser'sEye" style="zoom:60%;" />



**所以实际上，所谓这节的换入和下一节的换出，核心就是在映射上做文章**。而为什么要在映射上做文章呢？核心原因就是在用户的眼里有 4G 大的、规整的虚拟内存，但实际的物理内存可能根本没那么大，比如有的人是 1G 的内存，这时左边虚拟内存是 4G ，右边物理内存是 1G 。无论你的物理内存是 1G，2G，3G 等，不管你的物理内存是多大，给用户的感觉就是 [0, 4G] 的虚拟内存。

这种感觉让用户写起程序来非常方便，不用关心你的物理内存是多少，操作系统要隐藏计算机硬件使用的一些细节，方便用户使用。当物理内存大小和虚拟内存大小不一致，而虚拟内存随意地使用，那物理内存该怎么办呢？在这个例子中左边 4G，右边 1G，那怎么用 1G 的物理内存给用户提供有 4G 内存的感觉呢？给用户提供 4G 内存的视图呢？实际上，这种想法也不难，很容易想到，就是换入、换出，这在现实生活中非常常见。

请求的时候才做映射。用这种方法就可以实现 [0, 4G] 的一个规整的虚拟内存。请求的时候才换入内存，并且建立映射。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L24-UseSwappingInAndSwappingOutToAchieveLargeMemory.JPG" alt="L24-UseSwappingInAndSwappingOutToAchieveLargeMemory" style="zoom:60%;" />



请求，调入页面，建立映射或请求，换入，建立映射，怎么实现换入，核心就是一个东西，请求调页。接下来我们就来看它的实现。

当映射虚拟地址到页表中进行查找时，发现当前页表中没有这一页，这个时候操作系统就要开始忙了，要忙着干什么呢？要调页，当然这个页在哪里呢，在磁盘中。所以这也就形成了这样一幅图，把这个图理解了，实际上换入也就明白了，实际上还是非常简单的。

程序访问一个地址，之后一查页表发现缺页，一缺页会做什么事情呢？这个时候就要硬件帮我们做配合。执行一个地址，在这里执行不下去了，在后台做一会儿然后再回来，大家说这是什么概念，中断。当 MMU 查页表发现缺页时，就会打一个中断上去，还记不记得我们曾经讲过，CPU 上有个寄存器，当一缺页的时候就在这个寄存器上把一位置成 1。

每执行完一条指令，都要去看下有没有把那个寄存器上的一个 1 置上。当 MMU 产生了中断后，就要执行中断处理程序。中断处理程序从磁盘上找到这一页，怎么找到呢？肯定有数据结构、有算法可以找到。在磁盘上找到这一页后，就要在内存中找一个空闲页，get_free_page，然后把这一页从磁盘上读进来。

现在就有了请求调页，从磁盘上换入。接下来再把页表的映射更新好，然后中断处理完事，CPU 再开始执行刚才引起中断发生的那条指令。这个时候，当中断回来的时候，已经建立好新的映射了，已经换入了，那么就可以再执行这条指令，就没有问题了。

当然，从用户的角度来说的话，好像什么事都没发生过，因为整个换入过程对用户程序来说是完全透明的。所以从用户的角度来说，就感觉是执行一条指令稍微慢了点，过了一会儿又能继续执行。你无论访问 [0, 4G] 中的哪一段内存，至多是感觉慢了点（实际上后台去读页、换页去了），每一条指令都能够执行。

所以大家可以看到，应用这个换页（请求调页、页面换入），就可以实现用户能随意使用 [0, 4G] 的虚拟内存。这就是请求调页。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L24-DemandPaging-MMU-PageFaultInterrupt.JPG" alt="L24-DemandPaging-MMU-PageFaultInterrupt" style="zoom:60%;" />



在这个地方，除了请求调页以外，也可以做请求调段，这个也是可以的。但是在实际系统中，我们往往都用请求调页，而不用请求调段，这个是为什么呢？这里大家可以自己分析一下，我们把这个习题留作课下的练习题，大家认真地去思考思考。我们这个答案会在练习题中给出来。（选 C 的可能性较大）

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L24-WhyUseDemandingPagingNotDemandingSegmenting.JPG" alt="L24-WhyUseDemandingPagingNotDemandingSegmenting" style="zoom:60%;" />



明白了内存换入的过程，总之说来就一句话，通过换入，可以让用户感觉使用 [0, 4G] 的这样一个规整的虚拟内存，这就是实现了虚拟内存的概念。从用户的角度看起来 [0, 4G] 随便使用，只是有的时候稍微等一会儿。在等的过程中操作系统完成了换入，一换入内存后，就建立了新的映射，可以直接使用。

接下来我们就要看一个实际的系统，要看看它实际的代码，实际上这个代码实现也非常简单。

那么，这个实际系统请求调页的故事从哪里开始呢？从缺页中断开始。中断处理时首先设置中断，查查硬件手册，你的这个缺页打的是多少号中断，在这个例子中 page fault 打的是 14 号中断。在系统初始化的时候，就应该设置好 14 号中断由谁来处理。

实际上这一部分我们自己也可以想象得出来，无非不就实现换入的过程，就是从磁盘读进来放到内存中，然后再建立一个映射。而这样的事情怎么做呢？就是要 14 号中断的处理程序去做，那谁来设计 14 号中断的处理程序呢？在系统初始化的时候，在系统启动的时候 trap_init() 。

大家别忘了，操作系统最后是一个系统，我们一直等到学完以后，再给大家把所有的糅合在一起。实际上在这一讲，一开始的时候，我们就试图把内存管理的这几讲糅合在一起。实际上内存管理的这几讲，应该马上和 CPU 进程管理糅合在一起，然后再应该马上和系统调用、系统初始化糅合在一起。当然，如果再和后面的设备驱动、文件系统、磁盘管理糅合在一起，那么，一完整的操作系统就呼之欲出，马上就要出来了。

所以大家可以看到，要想做出操作系统来，就必须善于把各个模块糅合在一起。那么，在这个系统初始化的时候，我们可以设置 14 号中断，用这个去处理。怎么设置呢？这个前面讲过很多次了，无非就是初始化，修改 idt，中断向量表。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L24-DemandPagingOfAnActualSystem,WhereItBegin.JPG" alt="L24-DemandPagingOfAnActualSystem,WhereItBegin" style="zoom:60%;" />



接下来就是要 page fault，page fault 要做什么呢？因为中断了，要进入内核了，所以有一系列压栈、保存现场的行为。寄存器 ds、es 要变成 0x10（内核栈的选择子，表示到了内核数据段、代码段）。不过 cr2 赋给 edx 这条指令才是重点，cr2 表示页错误线性地址，页错误它一定有个错误地址，到底哪错了，这个错误地址就放在寄存器里。所以大家可以看到，CPU 的那些寄存器真的是非常、非常重要，cr0、cr3、cr2（这又出现了 cr2）等，要认识这些寄存器，要作为常识，这些常识没有的话，操作系统是做不出来的。

**在这课上，很多时候我们也把虚拟地址叫做线性地址，因为是 [0, 4G] 特别规整的地址，所以虚拟地址就是线性地址**。

接着把 edx 压栈。大家能不能想象，在这里有这样的敏感，一看到这个地方压栈，相当于把产生页错误的虚拟地址压栈，这要干什么呢？一个汇编语言把一个数字压栈，绝大多数都是要调用 C 函数，尤其是看到 call 的时候，就是要调 C 函数。那么也就是，调 C 函数的时候，你给 C 函数得传递参数，你得让它知道它哪错了、哪个地方缺页了，这样的话它才能处理。所以压入参数。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L24-HandlePageFault-PushParametersIntoStack.JPG" alt="L24-HandlePageFault-PushParametersIntoStack" style="zoom:60%;" />



address 这个虚拟地址传进来以后，我们就可以做 do_no_page 了。大家想想 do_no_page 要干什么，好好回顾下前面的图。不难想象，这个 do_no_page 要完成的事情是：现在这个虚拟地址对应的页不在内存中，因此要从磁盘上把这一页读进来，然后完成相应的映射关系。

所以，看到这个地方就是不详细去读代码，大家也应该能想象得到它要做这样一些事情：**读磁盘、在内存中申请一个空闲页、建立映射**。

那么，就来看看它核心代码是不是这个样子。实际上肯定是这个样子，如果我们自己写，也应该能写出来。

所以大家可以看到，不就是从磁盘上把你缺的那页找到，读到一个空闲的物理内存页上，然后 put_page，这句指令接下来会展开来说。

但是，在这个地方大家也可以想象得到要做些什么事呢？不就是把这个物理页和这个虚拟地址建立一个映射，实际上不就是填写页表嘛。所以大家可以看到，这三句话就构成了 do_no_page 的核心。实际上正好完成了刚才展示的那个动态的图，就是换入的过程、调页的过程，无非就是缺页中断的时候进行中断处理，处理的时候 do_no_page 就进行中断处理。

那处理的时候要干什么呢？**申请一个物理空闲页，然后从磁盘上读入这一页，接着建立这一物理内存页和虚拟地址的映射，然后再中断返回。之后再去执行引起缺页中断的那条语句时，由于已经建立好了新的映射，因此就一点问题也没有了**。所以大家可以看到，这些代码很容易就想到，也能够编写出来。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L24-do_no_page.JPG" alt="L24-do_no_page" style="zoom:65%;" />



看一下 put_page。我们刚才说过了，它应该就是修改页表。所谓建立虚拟地址和物理内存页之间的映射，不就是修改页表吗？大家可以看到，根据 address 找到页目录项，根据页目录项再找到页表项，然后把这个页表项和刚才传递进来的物理内存页的地址在页表中关联起来，相当于就建立了映射。这个函数里的其它代码都是为了实现最关键性的代码而服务，都很容易想到。

所以大家可以看到，为了实现页的换入、实现请求调页，它真正实现起来的代码也不困难。无非就是在中断处理中要做那三件事：**申请一个空闲页，在磁盘上把这一页读进来，再建立一个映射**。

这三件事做好了以后（在页面缺失的中断处理程序中去做这三件事），给用户的感觉是，无论你访问 [0, 4G] 的什么地方，都有。如果没有，它就进行缺页中断，等中断返回的时候这三件事已做完，它就已经有了。那么换页，请求换入，换页的换入部分就讲完了。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L24-put_page-page_table.JPG" alt="L24-put_page-page_table" style="zoom:60%;" />



&nbsp;

## L25 内存换出

上一讲我们讲的是内存的换入，这一讲我们讲内存的换出。实际上换入和换出是合在一起工作的。换入和换出的目的是干什么呢？就是要实现虚拟内存。比如虚拟内存空间有 4G，而物理内存只有 1G，如果仅是换入，一直往物理内存换入，那么内存迟早会满。

所以有换入，就必须得有换出。这样的话才能空出一些内存来让新的内容换入，这样的话才能让这个系统合理地工作，否则内存就会溢出。所以换入和换出必须合在一起工作，有换入就应该有换出。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L25-SwapInAndSwapOut.JPG" alt="L25-SwapInAndSwapOut" style="zoom:60%;" />



这个故事从 get_free_page 说起。get_free_page 是换入的时候得到一个物理空闲页，但并不是总有新的内存空闲页，因为内存是有限的。这时需要使用算法来选择内存中的哪一页被淘汰，换出到磁盘上。

接下来的内容主要是算法，采用一个什么样的算法来找页进行淘汰、换出呢？这三个算法非常简单，也很容易想到，顺理成章就可以想到。那么，最后主要的落处是 LRU 以及 LRU 算法的一个近似。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L25-BeginFromget_free_page.JPG" alt="L25-BeginFromget_free_page" style="zoom:60%;" />



接下来我们就来讲，有什么样的算法来选择哪一页进行淘汰。最开始想到的就是先入先出（FIFO）页面置换。每次我们讲这种调度的时候，都是从先来先服务这种最简单的开始，然后思考采用这种方法会产生什么样的问题，根据这个问题一点点延伸下去，提出更好的算法。

**这也是我们人的思维过程，大家不可能一下子就提出完美的算法，肯定是提出一个算法、改进，接着再提出一个算法、改进 ... ，最后才能出来实际的算法**。要体会这个特点，这一部分主要就是讲这个算法。

针对换出问题，我们给出的算法应该让缺页次数比较少。在给出一个算法时，分析这个算法在我们想要的指标下的特点、想要的这个指标下算法的表现，然后根据这个表现，分析它有没有问题，能不能改进，就这样的一个思路。这个也是所有做算法的一般性思路，这个没什么神奇的，

评价的准则是缺页次数。在这个实例中 FIFO 总共产生了 7 次缺页，前 3 次不可避免，每次缺页都要从磁盘上去读写。在第一个 D 到来时，不应该往前看，而应该往后看，在 D 的后面马上是 A ，所以这时换 A 出去是不合适的，B 也马上就会到来，相比之下 C 是将会来得最晚的。因此在 D 初次到来时，应该把 C 先换出去。这就引出了一种方法，MIN 页面置换法。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L25-FIFOPageReplacement.JPG" alt="L25-FIFOPageReplacement" style="zoom:60%;" />



MIN 算法：选最远将使用的页被淘汰，是最优方案。

在这个例子中，MIN 导致 5 次缺页，提升了性能。但是 MIN 需要知道将来发生的事，这个在实际中是用不了的。往后看是最优的，但是后面发生的事又不知道，那怎么办呢？未来不知道会发生什么呢，**但我们可以根据历史去推测未来，用过去的历史预测将来，这就是 LRU（Least Recently Used） 算法的核心观点**。

那么，怎么用过去来预测未来呢？实际上这也用到了我们前面所说的那个著名的局部性。我们知道，一个程序执行时，往往都是一段时间内都在一个内存范围内折腾，比如 while 循环。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L25-MINPageReplacement.JPG" alt="L25-MINPageReplacement" style="zoom:60%;" />



LRU 算法实际上就是对局部性这种规律的一种认识。那么实际上大家可以看到，**这个局部性的规律对于操作系统、对于计算机，它是非常重要的一个规律**。大家在做东西、做系统的时候，一定要抓住一些客观规律，就是我做的这个东西、这个对象所具有的一些客观规律。只有抓住了这些规律，才可以做出漂亮的、可以在实际中比较好使用的系统。

在这个实例中，LRU 也导致 5 次缺页，和 MIN 算法的效果是一样的。在实际系统中，通过做实验，LRU 算法的效果的确非常不错，是公认的很好的页置换算法。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L25-LRUPageReplacement.JPG" alt="L25-LRUPageReplacement" style="zoom:60%;" />



LRU 的准确实现，用时间戳

每页维护一个时间戳（time stamp），然后用这个数字，每次执行的时候选择数字最小的那个页被淘汰。这个算法好不好做呢？如果写成程序，如果学过数据结构、学过 C 语言，实现这个算法非常容易。但是，如果这个算法真放到操作系统里，却是不可行的。

这就是我们一再强调纸上得来终觉浅，觉知此事要躬行。往往一个东西要真正落到实际系统中的话，事情就变样了。模拟做一下，这个很容易，就是维护这几个数字就完事，但在实际系统中，那就很麻烦，而且需要很多的时间。每执行一条指令都要在这个表中找到相应的位置，要把表示时间戳的数字增大，而且还要处理它溢出的情况。每执行一条指令都要干这么多事情，而且这些事情都要访问内存，那么大家可想而知，每执行一条指令这个计算机慢多少。

所以 LRU 这种实现方法是不行的。模拟是一点问题也没有，就是我们编个小程序玩一玩是没问题，但放到操作系统中，是绝对不可行的，实现代价太大。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L25-ImplementLRUAccurately-TimeStamp.JPG" alt="L25-ImplementLRUAccurately-TimeStamp" style="zoom:60%;" />



LRU 准确实现，用页码栈。

每次地址访问都需要修改栈（修改 10 次左右栈指针），实现的代价较大，因此 LRU 的准确实现用得少，实际上在实际系统中都不用。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L25-ImplementLRUAccurately-PageNumberStack.JPG" alt="L25-ImplementLRUAccurately-PageNumberStack" style="zoom:60%;" />



LRU 近似实现，将时间计数变为是和否

讲到这个地方大家可以看到，绝知此事要躬行。对于 LRU 算法，你表面上想起来是那么一回事，理论上看起来挺合适的，但是你把它真正放到操作系统中去跑一遍，会发现根本就不是那么回事。

绝知此事要躬行，就是必须要扎进去以后，才明白是咋回事。所以 LRU 不能准确实现，要近似实现，怎么做到的呢？不去做时间戳，而是计一下计数，每个页加一个引用位（reference bit）。如果这一位是 1 的话，就再给一次机会，表示最近访问过；是 0  的话，就表示最近没有被使用。

**LRU 是最近最少使用，而 LRU 的近似是最近没有使用，把最近没有使用的页被换出去。最近没有使用，当然就是最近最少使用的一种近似**。

二次机会算法 SCR（Second Chance Replacement）也称为 Clock Algorithm 。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L25-ImplementLRUApproximately-ReferenceBit-1or0.JPG" alt="L25-ImplementLRUApproximately-ReferenceBit-1or0" style="zoom:60%;" />



Clock 算法的分析与改造

刚刚提到的那个 Clock Algorithm 虽然是 LRU 的一种近似，但是它的工作效果不好。待会儿你会马上看到它为什么不好。虽然它的效率很高，每次访问的时候只需要置下 0 或 1，但是它对 LRU 的近似不好。

通常缺页的情况很少，因为程序执行时有空间局部性的特点。如果一个程序执行时老缺页、换入换出的话，这个系统你不能要，或者说主要你内存买得少，你应该更新你的硬件，不然没办法再用下去。

如果缺页少的话，什么时候会把 1 置成 0 呢？只有当这一位变为 0 时这一页才有可能被淘汰。当缺页时会转动这个指针，把扫描到的页的 1 变为 0。当缺页的情况比较少时，1 就很少地变成 0。而如何把 0 变 1 呢？访问这一页的时候。

缺页少了，而这些页会被一直访问，这里面的 0 就会变成 1。当这里面的 0 变成 1 了，而缺页又很少了，不把 1 变成 0 了，那么最终的效果是什么样？这里面的 R 全为 1。

当这里的 R 全为 1 时，这个算法会怎么工作呢？指针会对页挨个扫描一遍，把所有的 1 变为 0 。全部变成 0  时会把谁换出啊？会把指针回到最开始指向的那个页换出。**大家想想，这个算法实际上变成什么算法了？退化成了 FIFO**，每次都顺序地把起始页一个个换出。一旦退化成了 FIFO，那么这个算法就没有 LRU 的思想在里面了，因此这时它的页面置换效果就不好。

造成这一现象的原因：因为缺页少，指针转的最近这段时间太长，它没法表达最近没有使用。在这一段较长的时间段内大家都使用了，所以它没有反映最近这个味道。**确切的说，就是这里的 R 记录了太长的历史信息，没法表达最近**。当 R 全变成 1 时，就退化成 FIFO。

要解决这个问题的核心，就在于要定时清除 R 位。再来一个扫描指针。

Clock 算法之所以叫 Clock 算法，因为它的这两个指针一快一慢，转来转去非常像表。讲到这个地方大家可以看到，我们从最直观的 FIFO ，到引出 MIN 算法，引出 LRU 算法的两种（时间戳和页码栈）准确实现，到 LRU 近似实现的二次机会算法 SCR（Clock Algorithm），然后又引出双指针的 Clock 算法。

当然，Clock 算法不只这一版本，有很多个版本，这个版本就可以工作。在实际系统中就变成了这样一个样子，把这个样子放在代码中，**比如说把定时清除的指针和时钟中断关联，把要置换页出去的指针和缺页中断处理程序关联，在 get_free_page 前面要有置换页的算法**。经过这些程序的相互配合，最后就实现了对 LRU 近似的 Clock 算法，这就是实现换出。讲到这个地方实际上换出基本上就完事了，但是要额外地解决一个附带的问题。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L25-ImproveClockAlgorithm-TwoScanPointer.JPG" alt="L25-ImproveClockAlgorithm-TwoScanPointer" style="zoom:60%;" />



到底应该给一个进程分配多少个物理内存页呢？大小应该适中，一旦把这个弄明白以后，知道要给一个进程分配多少个页，一个进程，再加上 Clock 算法中的指针转来转去，整个换出的全部故事就结束了。

在这个图中，**横轴、纵轴跟缺页没有任何关系，但它背后的根本原因就是缺页**。横轴是进程的个数，纵轴是 CPU 利用率。首先发现进程增多的时候，CPU 利用率会上来。这个可以想到原因，并发嘛，交替执行，CPU 很忙。但是，**当进程的数目多到一定程度的时候，CPU 的利用率突然急剧下降，这叫做系统出现了颠簸（thrashing）**。

为什么会出现颠簸呢？实际上是给进程分配的页少了。当进程太多的时候，给每个进程分配的页肯定就少了。每个进程分配的页少了，每个进程的缺页率就增加了。一旦缺页率增加，当增加到一定程度以后，就会产生这种现象，什么现象呢？

本来你应该用 4 个页，但只给你分配 3 页，在执行时刚把第 4 页置换出去，接着发现第 4 页马上又要使用了，然后又把第 4 页换进来。第 4 页换进来时，因为只给分配 3 页，那么把在内存中的这 3 页中某一页被置换出去，比如把第 1 页换出。那么过了一会儿，因为你总共需要 4 页，所以又需要把第 1 页换入。于是绕来绕去，不断地发生页的换入和换出。

一缺页就会调页，一调页就要启动磁盘，一启动磁盘 CPU 就要等着，这个进程等着，那个进程也等着，CPU 不执行实际的代码，CPU 的利用率会急剧地降低，所以这个时候系统会发生颠簸。

**什么叫颠簸呢，磁盘和内存之间不断地换入、换出页，但 CPU 啥也执行不了，只能等着**。当 CPU 啥也执行不了时，对用户来说，用户的程序得不到推进，用户就不满。所以系统一旦进入颠簸以后，效率就很低。

根据刚才可以看到，进程的个数不能特别多，个数多了就会颠簸。因为当分配给每个进程的页太少时，就会产生颠簸。太多和太少都不行，那么到底应该给进程分配多少个页框（frame）呢？应该给进程分配的页，能覆盖它在程序执行时的局部地址空间。但是这个局部到底是多大呢？while 循环到底有多长呢？当前程序是什么样呢？有没有 if 语句？这个很难说，所以这个局部求起来不那么容易。

有的书上有很多算法，比如求工作集，大家可以看下那样算法。如何求工作集，使用那个算法我们可以求一个局部，求出这个局部来就能明白给一个进程到底应该分配多少个页框。但这一部分我们这里不做讲解，一个基本的操作系统可以没有这一部分。

也就是说，我假定在系统中就给进程分配 20 个，如果觉得缺页比较多了，那我就多分配几个，如果缺页不频繁了，那我就少分配几个。所以究竟分配多少个页框，虽然理论上应该盖住程序执行时的局部内存空间，用工作集算法的思路。但是也可以做一些近似的算法，给它分配几个，然后通过不断地动态调整来做到。**总之分配给一个进程的页框个数应该调整，不然就会造成这种颠簸的现象，这就是核心**。

当然，产生颠簸的现象和进程有关。当进程的数目达到一定程度，怎么分配给各个进程的页框数，都覆盖不住它们各自执行时，需要的局部内存空间。因为这时进程实在太多了，所以要限制进程的数量。当然，这一部分已经是跟 CPU 管理、跟进程有关的内容。

总之大家可以看到，我们现在一旦确定好了给一个进程分配多少个页框，然后根据这些页框形成一个 Clock 的环形数组，利用 Clock 算法进行页面的换出、页面的淘汰、内存的换出。换出这一部分内容就全部结束、就讲完，形成了完整的故事。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L25-HowToDecideTheNumberOfPagesGivenToProcess-Thrashing.JPG" alt="L25-HowToDecideTheNumberOfPagesGivenToProcess-Thrashing" style="zoom:60%;" />



那么，我们再把上次课讲的换入和这次课讲的换出组合在一起。大家可以看到，换入呢，因为缺页。访问这个地址时发现缺页了，就进行缺页中断，从磁盘读一个页进来放入内存。当然，放入内存的过程中可能会发现分配给进程的页框数不足，这时就需要用 Clock 算法淘汰一个页框出去，swap out 出去一个页框，并把它写到磁盘上。写出去以后，一旦今后有空闲页的时候，可以再把它换入，swap in。

所以，一个 swap 分区就是既有换入又有换出，这就是著名的 swap 分区。大家在安装 Linux 的时候，都会让你安装 swap 分区，它的工作、要干的活、要干的事情就是这些样子。所以大家可以看到，换入有了，换出也有了，通过内存，当然这里还有时钟中断，缺页中断，磁盘，以及磁盘的管理，磁盘的驱动，这样就形成了一个 swap 的换入、换出的样子。

而实现换入换出的样子又是为了实现虚拟内存；而实现虚拟内存又是为了实现段、页；而实现段、页就是为了实现操作系统管理内存的思想；而管理内存呢，是为了让程序能够载入，让程序能够执行起来；而让程序能够执行起来，实现的是进程。所以大家可以看到，这就又落实到我们曾经讲的多进程图像。

所以，把这些所有东西合在一起，实际上在我们课程中间也讲过，**把这些所有东西合在一起，最终形成的就是一种以进程带动的、多进程推进的、内存开始有效工作的一张图**。这张图再加上初始化、系统接口这样一些辅助性的模块，再加上后面讲的设备驱动，整个计算机就开始向前运转，整个计算机的轮廓就有了。

之所以在这个地方说这段话，是因为到现在为止，到这一堂课为止，**我们讲完了内存管理，讲完了 CPU 管理，而内存管理、进程管理这两部分合在一起是操作系统的核心**。那么也就是说，到目前为止操作系统的核心就有了。

**操作系统的核心，再加上一些外围的设备驱动、文件系统这两部分（这是后面课程要讲的核心内容），再加上系统接口、系统初始化、系统引导，那么合在一起一个完整的操作系统就有了**。

如果你把这些内容，在头脑中能够形成一个完整的样子，形成一个轮廓，把这些内容深入理解了，掰碎了，揉烂了，把它理解了，消化了，那么我想你对操作系统的理解就上升了一个层次，对系统就有了一个完整的认识。那么你就能够，就有可能能够自己做出一个操作系统，至少会在一个实际的操作系统中，能够做一些更改，做一些模块性的设计和实现。

<img src="https://gitee.com/haokaimo/Picture/raw/master/OS-Lizhijun/L25-SwapIn-SwapOut-SwapPartitionManagement.JPG" alt="L25-SwapIn-SwapOut-SwapPartitionManagement" style="zoom:60%;" />



&nbsp;

## 附录

1. 实验部分可参考：[实验楼中配套的实验](https://www.shiyanlou.com/courses/115)；[hoverwinter/HIT-OSLab](https://github.com/hoverwinter/HIT-OSLab) 。
2. 课程的辅助教材：《Linux内核完全剖析》0.11 版（[豆瓣链接](https://book.douban.com/subject/1481173/) ；[oldlinux.org-clk011c-3.0.pdf](http://www.oldlinux.org/)）



&nbsp;

&nbsp;

> **版权声明：本文采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh) 许可协议，若转载请注明出处**。
