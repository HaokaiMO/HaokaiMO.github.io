---
title: '学习编程的正确姿势-牛岱'
category: Opinion
tags:
  - 'Learning Methods'
---

本文是我在 2019 年 10 月底听取牛岱的 [学习编程的正确姿势](https://www.zhihu.com/lives/1137494035894730752) 知乎 Live 讲座时，一边听录音一边记一些笔记后期整理完成的。我从牛岱的知乎账号上有了解到貌似他目前还是中国科学技术大学大四的学生，但我认为他的这个讲座里面讲的一些观点还是比较有参考价值的，所以也将他提到的一些内容整理出来，感兴趣的朋友也可以去购买听下他的这个知乎 Live 讲座。

<!-- more -->


## 1. GitHub 与 Stack Overflow 连接世界

GitHub 中
* Issue：评论/Bug 反馈；
* Pull Request：开源代码贡献；
* Git：版本控制/代码历史记录工具；
* Repository：代码仓库；
* Fork：克隆代码 ；


Stack Overflow
* 问答社区：提出你的问题、回答别人的问题；
* 几乎所有有价值的问题，容易遇到的问题，上面都有答案；

## 2. 搜索引擎的推荐  

我们希望搜索引擎能：

1. 广告可以有，但不要太多，避免充斥假冒伪劣网站；
2. 搜索结果尽量公平公正，不应照顾部分内容；
3. 能对英文搜索提供好的结果，不应该只是中文搜索引擎;

几大搜索引擎：Google、Bing (国内版/国际版) 、Quark (夸克) 。

## 3. 浏览器推荐

PC 端：Chrome、Edge。

移动端：Quark (夸克) 、Chrome。

搜索无处不在，PDF 阅览读书，写代码，浏览网页，搜索设置项，都要学会使用搜索。  Chrome 浏览器看文档时，`ctrl + f` 找相关的关键字。

搜索是面对大量信息快速获取目标信息的最有效方式。

## 4. 不同的身份认同，不同的行为模式

1. 学生 vs. 开发者；
2. 笔记本记笔记 vs. 写技术文章发表在知乎等平台；
3. 永远在准备，永远在夯实基础 vs. 在开发中遇到问题，扩展延伸学习，针对性强；
4. 遇到问题问老师，问同学 vs. 遇到问题 Google，Stack Overflow，Reference Docs；
5. 学了这么多知识，忘了怎么办 vs. 知识本来就是用进废退的，忘了就忘了呗；
6. 我一定要把 Java 基础打牢，在考试中取得高分，为以后的学习提供坚实的基础 vs. 我要在半年内通过学习 Java，运用 Java 做出一个中小型网站，基于 Web 的企业管理后台（**学习一门语言之前一定要知道它来干嘛！**）；
7. 学习就是要坚持，要做冷板凳，坚持是福，吃苦是福 vs. 一切不谈成就感，不谈反馈的学习都是在耍流氓；
8. 学生简历 vs. 开发者简历（**熟练使用XXX、精通XXX vs. GitHub 账号XX、XX项目作者**）；

## 5. 提到的一些观点

- 现实世界中的人处理现实世界中的问题，往往不是一口气完成所有知识准备，然后一口气顺畅的进行开发，进行实践。
- 现实世界是先进行最基础的知识储备，然后马上开始实践，实践中不断遇到各种问题，不断解决，以问题为中心延伸，完成细节知识的补充。
- 影响我最深的一句话：You can never understand everything. But, you should push yourself to understand the system.  Ryan Dahi (Creator of Node JS)
- 编程新手如何开始实践：克隆别人的项目 → 修改别人的项目 → 构建自己的项目。

## 6. 为什么要用命令行？

- 大部分工具没有图形界面；
- 做成图形界面可能会更复杂更难用；
- 命令行脚本，然后自动化执行；

## 7. 有哪些终端？

Windows
- Windows Terminal；
- Command Prompt；
- PowerShell；
- WSL；

Linux
- Bash；

## 8. 关于Git

- Git 工具：Branch；Commit；Remote/Local；
- Git 学习资源：[Become a git guru](https://www.atlassian.com/git/tutorials) (Learn Git with Bitbucket Cloud)；

## 9. 技术博客搭建

- 最容易（GitHub Pages）；
- 中等（WordPress 等建站工具）；
- 硬核（JHipster 功能应用）；

 JHipster 特别适合入门全栈开发，不仅是全栈开发，哪怕你现在是开发 Spring Boot 后端或 Vue 前端都很适合，它是一个代码生成器。在读懂生成的代码基础上，再修改。做 Web 开发的同学必须去了解。

## 10.  CheckStyle工具

现在几乎所有主流语言都有相应的代码风格检查工具，一般以 IDE 或 Editor 的插件或扩展形式给出。

## 11. 设计模式

设计模式并不是针对任何一种语言，而是一种用编程语言描述现实世界问题广泛采取的各种模式。比如观察者模式、工厂模式、依赖注入模式等。（这个 Live 讲座对应的 [GitHub 仓库](https://github.com/niudai/How-to-be-a-good-programmer) 里推荐了 2 个 YouTube 上的视频）

## 12. 一些小习惯

- 不要嫌变量名长，最好可以直接通过变量名推测变量的作用；
- 重复出现的代码，封装成独立的类或函数；
- 提前降低代码的耦合度，不同作用，不同类别的代码不要混合在一起，最好分成独立的文件；
- 将代码进行业务分层，比如在 Web 开发中，有数据层，服务层、DTO 层，Controller 层，渲染层等，只有将层次分开了，才能获得足够的可拓展性，不然代码多了你就全乱了；
- 用良好的设计模式去「设计」软件，在执行一些算法的时候可以想一想它的时空复杂度，想一想怎么可以让它执行地更快；
- 记得让 GitHub 托管你的项目哦~；

## 13. 计算机专业的四大核心课程

这四大核心课程建议大家好好学

- 算法与数据结构：我建议初、中级的学习你需要同时做两件事：
  - 学习 coursera 上开源《算法 4》课程：[教材](https://algs4.cs.princeton.edu/home/)，[视频教程](https://www.coursera.org/learn/algorithms-part1/)。
  -  同时做 leetcode 的 easy 到 medium 难度的题目，获得及时反馈，在讨论中学习优秀解法，体会算法与数据结构的用处。
- 操作系统
  - 我收集的两本经典教材（位于这个 Live 讲座对应的 [GitHub 仓库](https://github.com/niudai/How-to-be-a-good-programmer) 中 CS textbooks 文件夹下）: Operating System Concept、Linux Kernel Development 。
  - 推荐一个自我实践制作的一个操作系统 GitHub 项目：[How-to-Make-a-Computer-Operating-System](https://github.com/SamyPesse/How-to-Make-a-Computer-Operating-System) 。
  - 操作系统对绝大多数程序员来讲你最重要的事情是去理解它，理解它整个设计思路，比如说 CPU 是如何不断去切换每个进程的。

- 计算机组成原理
- 计算机网络

## 14. 包管理工具

- Linux：apt-get、yum、etc. ;
- OSX：brew ;
- Windows：Chocolatey ;

## 15. 开发工具

- Visual Studio：C；C++；
- Visual Studio Code：除了 C/C++ 外的几乎所有主流语言；

## 16. 扩展视野

- Twitter；
- YouTube：微软 builder 大会等
  - Youtube 上推荐的一些频道 
    - CodeOrg（质量超高的科普视频，讲解计算机运行原理，尤其适合入门。而且讲解的都是大牛，讲得非常好懂）。
    - Computerphile（提升视野利器，擅长一些计算机概念讲解，和一些技巧科普，比如数据分析可视化技巧，SQL 注入攻击等，涉猎广泛，且十分有趣）。
    - Java 程序员学习网站：Telusko（从 Spring MVC 到 Spring Boot，再到设计模式，一系列教程简直是 Java 程序员的福音。而且讲得很形象，很好懂，但不建议英语差的同学听，因为他语速快，发音不清，可能初学者听不太懂他说话，但一旦听懂了就能感觉很棒了）。
    - 编程语言知识百科：Derek Banas（从 Java 到 C#，再到游戏开发，再到 JavaScript，再到日语，再到设计模式，再到前端框架，再到 Java EE，再到 Matlab，这兄弟目前为止在不下十门计算机语言领域制作了质量超高的教程，在油管上也是被当作神一样的存在，大家可以瞻仰一下他的播放列表，看看有多么恐怖~）。

## 17. 一些提醒

- 不要碰一些二三流的代码，最好读的代码、参考的项目是被很多人承认的好项目，不要去弄那种乱七八糟也不知道是谁做的项目，可能有很多坑。用库的话不要用那种还没有被测试过的，可能有未知 bug 的库。
- 改变非黑即白的思维，很多事情可以同时进行，你完全可以每天三小时间去学算法和数据结构，然后抽三个小时时间去做你的 C 语言小游戏。
- 学习的过程不是线性的，你们有没有发现凡是能线性的、很顺利的从头到尾一个个学下来的都很简单。
- 我学习过程中有一个习惯，当我看一篇文章或者读一本教材时，当我读不懂的时候，我从来不会怀疑自己的学习能力，我只会去思考两件事，第一，是不是这本教材讲得不太好，讲得不清楚，第二件事是是不是自己缺少了某些前置知识。看教材我从来不先看学校发的教材，自己先去搜公认的较好的教材是什么，有时候我学得比较快，比较好，也是因为我看得教材讲得就比较明白。
- 有些知识点是孤立的点，比如汉语中的生僻字；而有些知识点是相互关联的，比如 HTTP，前后端都得学习、了解。 

## 18. 互动回答

如果你是以就业为导向的话，市场上需求最大的应该还是 Web 开发。AI，算法工程师竞争最大（很多大学生他们喜欢去研究那个算法，但工程能力不行，他们自己不做项目，他们就做那个算法题，参加 ACM 竞赛，所以算法好的同学特别多，竞争就比较大，你可以就是着重提升一下自己的工程能力，工程能力强的学生我觉得也是特别受欢迎的）。

## 19. 附录

1. 这个 Live 讲座对应的 [GitHub 仓库](https://github.com/niudai/How-to-be-a-good-programmer)；
2. 关于命令行：[GitHub 六万星项目: the-art-of-command-line (墙裂推荐！)](https://github.com/jlevy/the-art-of-command-line)；
3. 这个 Live 讲座的 GitHub 仓库中 Issues 中的内容:
   - [廖雪峰 Git 教程](https://www.liaoxuefeng.com/wiki/896043488029600)
   - [一个关于廖雪峰 Git 教程的整理](https://github.com/Zhangguoliu/learn-git/blob/master/learngit-note.md)

&nbsp;

&nbsp;

> **版权声明：本文采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh) 许可协议，若转载请注明出处**。
