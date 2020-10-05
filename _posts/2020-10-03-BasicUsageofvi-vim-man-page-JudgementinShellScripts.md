---
title: 'vi/vim，man page，Shell Scripts 判断式的基本使用'
category: Linux
tags: 
  - 'vi/vim'
  - 'man page'
  - 'Shell Scripts 判断式'
---

这篇文章汇整了鸟哥的 Linux 私房菜中，对 vi/vim、man page 以及 Shell Scripts 判断式的基本使用方式的总结。其中主要是鸟哥整理的表格，我以图片的形式汇整到一起以方便今后查阅。

<!-- more -->



&nbsp;

## vi



### 一般指令模式（command mode）



<img src="https://gitee.com/haokaimo/Picture/raw/master/Linux-VbirdTsai/vi-CommandMode-00.JPG" alt="vi-CommandMode-00" style="zoom:70%;" />

<img src="https://gitee.com/haokaimo/Picture/raw/master/Linux-VbirdTsai/vi-CommandMode-01.JPG" alt="vi-CommandMode-01" style="zoom:70%;" />

<img src="https://gitee.com/haokaimo/Picture/raw/master/Linux-VbirdTsai/vi-CommandMode-02.JPG" alt="vi-CommandMode-02" style="zoom:70%;" />

<img src="https://gitee.com/haokaimo/Picture/raw/master/Linux-VbirdTsai/vi-CommandMode-03.JPG" alt="vi-CommandMode-03" style="zoom:70%;" />



&nbsp;

### 编辑模式（insert mode）



<img src="https://gitee.com/haokaimo/Picture/raw/master/Linux-VbirdTsai/vi-InsertMode-00.JPG" alt="vi-InsertMode-00" style="zoom:70%;" />



&nbsp;

### 指令列命令模式（command-line mode）



在一般模式中，输入「 : , / , ? 」三个中的任何一个按钮，就可以将光标移动到最底下那一列，进入指令列命令模式。按 Esc 回到一般指令模式。



<img src="https://gitee.com/haokaimo/Picture/raw/master/Linux-VbirdTsai/vi-Command-lineMode-00.JPG" alt="vi-Command-lineMode-00" style="zoom:70%;" />

<img src="https://gitee.com/haokaimo/Picture/raw/master/Linux-VbirdTsai/vi-Command-lineMode-01.JPG" alt="vi-Command-lineMode-01" style="zoom:70%;" />



特别注意，在 vi 中，数字是很有意义的！数字通常代表重复做几次的意思！也有可能是代表去到第几个什么什么的意思。举例来说，要删除 50 列，则是用「50dd」对吧！数字加在动作之前~ 那我要向下移动 20 列呢？那就是「20j」或者「20↑」即可。

OK！会这些指令就已经很厉害了，因为常用到的指令也只有不到一半！通常 vi 的指令除了上面鸟哥注明的常用的几个外，其它是不用背的，你可以做一张简单的指令表在你的屏幕墙上，一有疑问，可以马上地查询呦！这也是当初鸟哥使用 vim 的方法啦！



&nbsp;

## vim



### 区块选择（Visual Block）



<img src="https://gitee.com/haokaimo/Picture/raw/master/Linux-VbirdTsai/vim-VitualBlock.JPG" alt="vim-VitualBlock" style="zoom:70%;" />



&nbsp;

### 多文件编辑



可以用 `vim /etc/hosts hosts` 这个指令来用 vim 开启两个文件。

<img src="https://gitee.com/haokaimo/Picture/raw/master/Linux-VbirdTsai/vim-EditMutifile.JPG" alt="vim-EditMutifile" style="zoom:70%;" />



&nbsp;

### 多窗口功能



在一般指令模式下输入 `:sp {filename}` ，即可在 vim 中开启另一个窗口，其中这个 filename 是可有可无的，如果想要在新的窗口中启动另一个文件，就加入文件名。否则仅输入 `:sp` 命令执行后，出现的是同一个文件在两个窗口中。

分区窗口的相关指令功能有很多，不过你只要记得这几个就好了：

<img src="https://gitee.com/haokaimo/Picture/raw/master/Linux-VbirdTsai/vim-Multiwindow.JPG" alt="vim-Multiwindow" style="zoom:70%;" />



&nbsp;

### vim 挑字补全功能



<img src="https://gitee.com/haokaimo/Picture/raw/master/Linux-VbirdTsai/vim-Auto-complete.JPG" alt="vim-Auto-complete" style="zoom:70%;" />



在鸟哥的认知中，比较有用的是第 1，3 这两个组合键。在第 1 个组合按键中，你可能会在同一个文件里面重复出现许多相同的关键字，那么就能够通过这个补全的功能来处理。如果你是想要使用 vim 内建的语法检验功能来处理取得关键字的补全，那么第 3 个组合键就很有用了。不过要注意，如果你想要使用第 3 个功能，就得要注意你编辑的文件的扩展名。



&nbsp;

### vim 环境设定与记录



vim 会主动将你曾经在用 vim 打开的文件中，做过的行为记录下来，好让你下次再用 vim 打开时能更轻松地使用。这个记录动作的文件就是 ~/.viminfo 。如果你曾经使用过 vim，那你的 home 目录下应该会存在这个文件才对。这个文件是自动产生的，你不必自行建立。而你在 vim 中所做的动作，就可以在这个文件内部查询到啰~

vim 的环境设定参数有很多，如果你想知道目前的设定值，可以在一般指令模式时输入 `:set all` 来查阅。不过，设定项目实在太多了，所以鸟哥在这合理仅列出一些平时比较常用的一些简单的设定值，提供给你参考。

<img src="https://gitee.com/haokaimo/Picture/raw/master/Linux-VbirdTsai/vim-EnvironmentSetting-00.JPG" alt="vim-EnvironmentSetting-00" style="zoom:70%;" />

<img src="https://gitee.com/haokaimo/Picture/raw/master/Linux-VbirdTsai/vim-EnvironmentSetting-01.JPG" alt="vim-EnvironmentSetting-01" style="zoom:70%;" />



所谓的缩排，就是当你按下 Enter 编辑新的一列时，光标不会在行首，而是在与上一列的第一个非空格符处对齐。

我们可以通过配置文件来直接规定我们习惯的 vim 操作环境。整体 vim 的设定值一般是放置在 /etc/vimrc 这个文件，不过，不建议你修改它！你可以修改 ~/.vimrc 这个文件（默认不存在，请你自行手动创建！），将你所希望的设定值写入。举例来说，可以是这样的一个文件：

<img src="https://gitee.com/haokaimo/Picture/raw/master/Linux-VbirdTsai/vim-EnvironmentSetting-example.JPG" alt="vim-EnvironmentSetting-example" style="zoom:80%;" />



在这个文件中，使用 `set hlsearch` 或 `:set hlsearch` ，亦即最前面有没有冒号 `:` 效果都是一样的！至于双引号则是注解符号，不要用错注解符号，否则每次使用 vim 时都会发生警告讯息喔！

建立好这个文件后，当你下次重新以 vim 编辑某个文件时，该文件的默认环境设定就是上头写的啰~ 这样，是否很方便你的操作啊！多多利用 vim 的环境设定功能呢！



&nbsp;

### vim 常用指令示意图



为了方便大家查询在不同模式下可以使用的 vim 指令，鸟哥查询了一些 vim 与 Linux 教育训练手册，发现地下这张图非常值得大家参考！可以更快速有效地查询到需要的功能喔！

<img src="https://gitee.com/haokaimo/Picture/raw/master/Linux-VbirdTsai/vim-FrequentlyUsedButton.JPG" alt="vim-FrequentlyUsedButton" style="zoom:80%;" />



&nbsp;

## man page



### 4.3.2 man page



使用 man 查询指令时，指令名称旁边的括号中数字的含义，例如 DATE(1) 。

<img src="https://gitee.com/haokaimo/Picture/raw/master/Linux-VbirdTsai/man-page-Number-types.JPG" alt="man-page-Number-types-cn" style="zoom:64%;" />

上表中的 1，5，8 这三个数字所代表的含义比较重要，最好能背下来。



基本上 man page 大致分成底下这几个部分：

<img src="https://gitee.com/haokaimo/Picture/raw/master/Linux-VbirdTsai/man-page-content.JPG" alt="man-page-content" style="zoom:70%;" />

有时候除了这些外，还可能会看到 Authors 与 Copyright 等，不过也有很多时候仅有 NAME 与 DESCRIPTION 等部分。通常鸟哥在查询某个资料时是这样来查阅的：

1. 先查看 NAME ，约略看一下这个数据的意思；
2. 再详看一下 DESCRIPTION ，这个部分会提到很多相关的数据与使用时机，从这个地方可以学到很多小细节呢；
3. 而如果对这个指令其实很熟悉了，比如 date，那么鸟哥主要就是查询关于 OPTIONS 的部分了，可以知道每个选项的意义，这样就可以下达比较细部的指令内容呢；
4. 最后鸟哥会再看一下，跟这个数据有关的还有哪些东西可以使用的？比如 SEE ALSO 部分。此外，某些说明内容还会列举有关的文件（FILES 部分）来提供我们参考，这些都是很有帮助的！



man page 常用的按键：

<img src="https://gitee.com/haokaimo/Picture/raw/master/Linux-VbirdTsai/man-page-KeyFunction.JPG" alt="man-page-KeyFunction" style="zoom:70%;" />



一般来说鸟哥是真的不会去背指令的，只会去记住几个常用的指令而已。那么鸟哥是怎么找到所需要的指令呢？举例来说，打印的相关指令，鸟哥其实仅记得 lp (line print) 而已。那我就由 man lp 开始，去找相关的说明，然后再以 lp\[tab][tab] 找到任何以 lp 开头的指令，找到我认为可能有点相关的指令后，先以 -\-help 去查基本的用法，若有需要再以 man 去查询指令的用法！所以，如果是实际在管理 Linux，那么真的只要记得几个很重要的指令即可，其它需要的，嘿嘿！努力地找男人（man）吧！



&nbsp;

## Shell Scripts 判断式



### 12.3.1 利用 test 指令的测试功能



<img src="https://gitee.com/haokaimo/Picture/raw/master/Linux-VbirdTsai/Linux-test-00.JPG" alt="Linux-test-00" style="zoom:70%;" />

<img src="https://gitee.com/haokaimo/Picture/raw/master/Linux-VbirdTsai/Linux-test-01.JPG" alt="Linux-test-01" style="zoom:70%;" />

<img src="https://gitee.com/haokaimo/Picture/raw/master/Linux-VbirdTsai/Linux-test-02.JPG" alt="Linux-test-02" style="zoom:70%;" />

除了 test 之外，我们还可以利用判断符号 []（就是中括号啦）来进行数据的判断。使用中括号必须要特别注意，因为中括号用在很多地方，包括通配符与正则表达式等，如果要在 bash 的语法当中使用中括号作为 shell 的判断式时，必须要注意中括号的两端需要有空格符来分隔喔！

利用 [] 来进行判断时，还要注意：

- 在中括号 [] 内的每个组件都需要有空格键来分隔；
- 在中括号内的变量，最好都以双引号括起来；
- 在中括号内的常数，最好都以单引号或双引号括起来。

此外，利用 [] 来进行判断时，使用的方式与 test 几乎一模一样！





&nbsp;

&nbsp;

## 参考资料



1. 鸟哥的 Linux 私房菜-基础篇第四版（链接：[鸟哥的Linux私房菜-基础学习篇目录](http://linux.vbird.org/linux_basic/) ）



&nbsp;

&nbsp;

> **版权声明：本文采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh) 许可协议，若转载请注明出处**。
