---
title: '使用GitHub Pages + Jekyll搭建个人博客的参考'
category: Reference
tags: 
  - 'GitHub Pages'
  - 'Jekyll'
---

这篇文章记录了最近几个星期以来，我从对 GitHub Pages 完全陌生，到知道了如何使用 GitHub Pages + Jekyll 来搭建个人博客的这个过程中，通过一些搜索所查看的相关参考资料。文章中不会对搭建个人博客的步骤进行详细说明，主要是从时间顺序上列出我在这一探索过程中所查看的资料，因为假如你在大致看了这些参考资料后，如果有搭建个人博客的想法，那么我想你应该知道下一步如何具体去做。

<!-- more -->

## （一）

之前有了解到使用 GitHub Pages 来搭建个人博客是一种较简单的方式，因为它不需要花时间去折腾服务器、域名等，对于目前我想把自己学习过程中的一些笔记或体会放到博客页面上的这一需求来说完全够了。于是我首先在 GitHub 官网上的 GitHub Guides 查看了 [Getting Started with GitHub Pages](https://guides.github.com/features/pages/) ，在查看的过程中也在 [GitHub Pages 官网](https://pages.github.com/) 上看了首页的短视频和相应的文字介绍，然后便对 GitHub Pages 有了一个总体的了解。接着我参加了 GitHub Learning Lab 上的 [GitHub Pages](https://lab.github.com/githubtraining/github-pages) 课程，按照操作说明完成了相应的练习，至此我知道了通过一些简单的设置，可以使用 GitHub Pages 来产生一个简单的页面，但对 Jekyll 还没有进行学习与了解。

其实在上述过程中，在查看 [GitHub Pages 官网](https://pages.github.com/) 上的介绍时就有看到 Jekyll 这个词，因为 GitHub Pages 是默认支持 Jekyll 的。而前一段时间在阳志平的博客上 [如何高效利用GitHub](https://www.yangzhiping.com/tech/github.html) 这篇文章中也提到了使用 GitHub Pages + Jekyll 来进行博客写作，文章内容中有提到 [Jekyllbootstrap](http://jekyllbootstrap.com/)，我在点击这个链接后，在打开的页面介绍中也点击了 [Jekyll Introduction](http://jekyllbootstrap.com/lessons/jekyll-introduction.html) 的链接，接着打开了一个关于 Jekyll 介绍的页面内容，不过当时我并没有马上进行详细阅读这些介绍 Jekyll 的文字，想着到时候先看下  [Jekyll官网](https://jekyllrb.com/)  上的一些介绍后再回过头来看这些内容。在看完阳志平的「如何高效利用GitHub」博客后，我还看了他的另一篇文章——  [理想的写作环境：Git+GitHub+Markdown+Jekyll](https://www.yangzhiping.com/tech/writing-space.html)， 在这篇文章中他对标题的这一观点进行了一些简短的介绍，其中有提到 [Blogging Like a Hacker](http://tom.preston-werner.com/2008/11/17/blogging-like-a-hacker.html) 。

后来我通过在知乎上搜索 GitHub Pages + Jekyll 关键字后了解到一个观点——你可以在 GitHub 上先 Fork 他人使用 Jekyll 搭建个人博客的仓库，然后再改成你自己的博客仓库。于是在大致知道是怎么一回事后，我在 [Jekyll官网](https://jekyllrb.com/) 上开始对 Jekyll 进行了解。首先是查看 [Jekyll DOCS](https://jekyllrb.com/docs/) 中的 Quickstart 的介绍，然后根据相应的说明在 Windows 10 中通过 [RubyInstaller](https://rubyinstaller.org/) 来安装 Ruby 的运行环境，因为只有在具备 Ruby 的运行环境后才能在本地上进一步使用 Jekyll。之后通过 [RubyInstaller](https://rubyinstaller.org/) 下载了相应的安装包后，通过在 Google 上搜索 Windows 10 上安装 Ruby 的介绍说明来进行安装。

安装完成后便根据 Jekyll 官网上的 [Quickstart](https://jekyllrb.com/docs/) 上的操作说明以及 [Jekyll Tutorials -- Video Walkthroughs](https://jekyllrb.com/tutorials/video-walkthroughs/) 这一系列视频中前面几个安装与启动的视频介绍进行相应的操作，但在执行 `bundle exec jekyll serve` 这一命令进行页面本地启动时，我当时遇到了 Permission denied - bind(2) for 127.0.0.1:4000 (Error:EACCES) 这一异常情况，后来了解到是因为电脑上 4000 的端口被占用了，于是通过在 cmd 中执行 `netstat -ano|findstr "4000"` 来找到当前使用 4000 端口进程的 PID 数字（或者使用 `tasklist|findstr 4000` 查找占用 4000 端口号的进程名），接着打开 Windows 任务管理器窗口，在详细信息 Tab 中找到相应的进程后，结束该进程，然后再执行 `bundle exec jekyll serve` 进行启动，正常的话根据命令执行后的提示—— Server address: http://127.0.0.1:4000，在浏览器中输入 http://127.0.0.1:4000 进行访问后就能查看所生成的页面。

接着我进一步在  [Jekyll DOCS](https://jekyllrb.com/docs/) 中了解了 Gemfile、Bundler、Liquid、Front Matter、Layouts、Includes、Data Files、Assets、Blogging、Deployment，然后也把  [Jekyll Tutorials -- Video Walkthroughs](https://jekyllrb.com/tutorials/video-walkthroughs/) 这一系列讲解的视频看了，并在 GitHub Help 上把 [Setting up a GitHub Pages site with Jekyll](https://help.github.com/en/github/working-with-github-pages/setting-up-a-github-pages-site-with-jekyll) 这个页面中所链接的相关文章大致浏览了下。

在完成这些内容后，我开始对之前找到的 [ishanshan](https://github.com/ishanshan/ishanshan.github.io) 和 [Simpleyyt](https://github.com/Simpleyyt/simpleyyt.github.io) 这两个使用 [jekyll-theme-next](https://github.com/Simpleyyt/jekyll-theme-next) 模板的博客仓库内容进行对比和分析，然后通过修改 _config.yml、404.html、Gemfile文件以及 _sass、 _includes 、assets 等文件夹中的内容进行个性化的设置，比如调整页面字体大小、页面显示 Markdown 文章大纲 toc 高亮的颜色等。最后在 _posts 文件夹中按相应的规则放入博客文章—— .md 文件后，在命令行窗口 cmd 中进入本地存放博客页面的文件夹下，执行 `bundle exec jekyll serve` 进行页面启动，正常运行后根据 cmd 窗口中提示的信息，在  http://127.0.0.1:4000 地址中就可以预览所生成的博客页面以及放置在 _posts 文件夹中 Markdown 文章。

在本地预览所生成的页面后，如果确认没有问题，那么便可以把本地存放博客的文件夹中除了 _site 文件夹（ _site 文件夹的内容是 Jekyll 根据你存放博客的文件夹中其它文件自动生成的）都推送到你的 GitHub 账户中存放个人博客的仓库中，推送成功后等几十秒样子就可以在类似 https://username.github.io 地址中访问你的博客页面，此外 GitHub Pages 也支持你修改个人博客的域名。

## （二）

上面的介绍就是我在使用 GitHub Pages + Jekyll 来搭建个人博客这一过程中的大致经过，这一部分是我在搭建博客过程中其它一些细节内容。在前面一部分中我有提到当时在知乎上搜索了 GitHub Pages + Jekyll 关键字，在查看搜索结果时，我主要查看了以下三篇文章的介绍：

1. [Github Pages + jekyll 全面介绍极简搭建个人网站和博客](https://zhuanlan.zhihu.com/p/51240503) 
2. [技术人如何搭建自己的技术博客](http://www.ityouknow.com/other/2018/09/16/create-blog.html) 
3. [可能是最全面的github pages搭建个人博客教程](https://lemonchann.github.io/create_blog_with_github_pages/) 

之后在我使用 [jekyll-theme-next](https://github.com/Simpleyyt/jekyll-theme-next) 这一模板来生我的博客页面时，发现显示文章内容的页面底部 Disqus 评论部分没有加载出来。后来我发现需要先在 [Disqus官网](https://disqus.com/) 上进行账号注册，注册后按照页面上的相关说明进行操作，然后再把 Disqus 加载到你的博客网站中。在这期间你会在 Disqus 上设置一个你的博客网站的名称，记住这个名称后再把 _config.yml 文件中 disqus 下 shortname 的值设置为这个名称，接着执行 `bundle exec jekyll serve` 重新启动博客页面后就能加载出 Disqus 评论模块了。

在为我的 GitHub 账号上的博客仓库选择开源协议以及我的博客文章选择知识共享协议时，我主要参考了以下三个内容：

1. [程序员不可不知的版权协议](https://www.gcssloop.com/tips/choose-license?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io) 
2. [Choose an open source license](https://choosealicense.com/) 
3. GitHub Help 上的 [Licensing a repository](https://help.github.com/en/github/creating-cloning-and-archiving-repositories/licensing-a-repository) 

在我的博客页面生成后，我通过在 [百度统计](https://tongji.baidu.com/web/welcome/login) 上注册了站长版的账号，然后把我的博客网站添加了上去，在添加成功后会自动生成一小段代码，从生成的这段代码中复制 https://hm.baidu.com/hm.js? 后面那一串数字，然后再把 _config.yml 文件中 baidu_analytics 的值设置为这串数字（可以参考 [第三方服务集成-NexT使用文档](http://theme-next.simpleyyt.com/third-party-services.html)、[给Hexo主题博客加入百度站点统计](https://blog.csdn.net/Sophie_U/article/details/84326459) ）。在本地文件修改后若没什么问题，再把修改后的文件推送到 GitHub 相应的远程仓库就可以了。

&nbsp;

&nbsp;

> **版权声明：本文采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh) 许可协议，若转载请注明出处**。

