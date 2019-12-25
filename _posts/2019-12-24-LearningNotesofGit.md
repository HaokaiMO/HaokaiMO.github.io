---
title: 'Git学习笔记'
category: Tool
tags: 
  - 'Git'
---

这篇文章是我在学习 Git 过程中，通过查看和练习相关的学习资料时所做的一些个人总结，运行的系统环境是 Windows 10。文章主要可分为两个部分，一部分是对安装 git for Windows 进行一些简要说明，然后使用 Git Bash 进行初步设置，并与个人的 GitHub 账号关联起来；另一部分是在学习 [廖雪峰的 Git 教程](https://www.liaoxuefeng.com/wiki/896043488029600) 时所记录的要点内容。

<!-- more -->

在 Windows 上使用 Git 可以去 [Git官网](https://git-scm.com/) 下载 git for Windows，这个软件的安装包在进入官网的下载页面时会自动下载，在软件安装的过程中基本可以使用安装导向中默认的选项，但有几处设置需要注意：

1. 设置默认编辑器时（choosing the default editor）如果你不熟悉 Vim，推荐使用 notepad++，如果 notepad++ 电脑上还没有安装过，可以先退出 git for windows 的安装导向，安装完 notepad++ 后再重新运行 git for windows 安装包进行安装。
2. 在设置路径环境时（path environment）推荐使用默认的 use git from the command line and also from 3rd-party software，这一选项可以让你从 Git Bash 或 Windows Command Prompt 使用 Git。
3. 安装导向的其余设置中可以陆续选择 Use OpenSSH、Use the OpenSSL Library (in choosing HTTPS transport backend)、Checkout Windows-style, commit Unix-style line endings (in the Configuring the line ending conversions)、Use MinTTY (the default terminal of MSYS2) (in the Configuring the terminal emulator to use with Git Bash)，最后的 Configuring extra options 选项中除非你需要 symbolic links，否则使用默认的两个选项就可以进行安装了。

## 设置用户名和用户邮箱

在打开的 Git Bash 窗口中输入：

`git config --global user.name "<your name>"`

在使用 GitHub 作为远程的代码仓库时，这里的 `<your name>` 为你的 GitHub 账户的用户名，命令输入后按 enter 键进行执行，接着输入：

`git config --global user.email "<your email>"`

同样 `<your email>` 为你注册 GitHub 账号的邮箱。

## 从你的GitHub账号中clone一个repository到本地

在 Git Bash 中用 cd 命令进入到一个你想保存 Git 文件仓库的文件夹，输入：

`git clone <URL>`

其中 `<URL>` 是你 GitHub 账号中已有的一个 repository 的地址，在该 repository 页面中点击 clone or download 可以看到 https 开头的地址，点击右边的按钮进行复制。在 clone 命令成功执行后，你将在之前由 cd 命令进入的本地文件夹中有一个和你 GitHub 账号中由这个 `<URL>` 地址所代表的 repository 名称相同的文件夹。之后输入：

`git remote` 

执行后将会在 Git Bash 中看到 `origin` 字样，它表示的是远程仓库的名称。接着输入：

`git remote -v`

命令执行后将会看到 fetch 和 push 的两个 https 地址。

下面的这一部分是我根据 [廖雪峰的Git教程](https://www.liaoxuefeng.com/wiki/896043488029600) 进行练习学习时整理的相关要点。

## 廖雪峰Git教程

### 1 创建版本库（repository）

#### 1.1 在本地创建一个版本库

首先在 Git Bash 中用 cd 命令进入一个文件夹，然后用 mkdir 命令创建一个空的目录作为 Git 的本地repository，输入：

```
mkdir learnGit
cd learnGit
pwd
```

`pwd` 命令用于显示当前的目录（print working directory），之后通过输入：

`git init`

执行该命令后把这个目录变成 Git 可以管理的仓库，执行完这个命令后可以发现在当前目录下多了一个隐藏的 .git 目录，在 Git Bash 中也可以输入 `ls -ah` 命令进行显示隐藏的文件。

#### 1.2 把文件添加到版本库

在当前 learnGit 目录下，使用 notepad++ 采用默认的 UTF-8 without BOM 编码方式新建一个 readme.txt 的文件，内容可以是

```
Git is a version control system.
Git is free software.
```

接着输入：

`git add readme.txt`

命令执行后把文件添加到 Git 仓库，然后再输入：

`git commit -m “wrote a readme file”`

命令执行后就把上一步添加的文件提交到 Git 仓库中，`-m` 后面的字符串代表的是提交说明。Git 中 add 命令可以多次 add 不同的文件，然后用 commit 进行一次提交，比如

```
git add fileOne.txt
git add fileTwo.txt fileThree.txt
git commit -m “add 3 files”
```

在本地仓库中由 Git 维护的三颗「树」组成，第一个是**工作目录**；第二个是**暂存区**；第三个是**HEAD**，它指向你最后一次提交的结果，它们的关系如下图所示（图片来自 [git 简明指南](http://rogerdudler.github.io/git-guide/index.zh.html) 页面中的内容）：

<img src="https://raw.githubusercontent.com/HaokaiMO/hello-world/master/LearningNotesofGit/git-workingFlow-three-tree.JPG" width="584" height="375" alt="git-workingFlow-three-tree" />

### 2 时光穿梭

执行 `git status` 可以查看仓库当前的状态，如果对 readme.txt 进行一些修改保存后再输入 `git status` 执行后可以在输出的内容中看到 modified: readme.txt 的信息，并提示我们没有还没有将修改后的 readme.txt 添加到缓存区准备进行提交。输入：

`git diff readme.txt`

可以查看修改后和修改前的文件对比，输出的格式是 Unix 通用的 diff 格式。接着对修改后的 readme.txt 进行 `git add` 和 `git commit` 操作，再输入 `git status` 执行命令后此时 Git Bash 会显示当前没有需要提交的，工作目录是干净的。

#### 2.1 版本回退

接着对 readme.txt 再进行一次修改，然后依次用 `git add`、`git commit` 进行添加和提交。这样我们已经通过 `git commit` 提交了好几次，有时候可能记不清每次都提交了哪些内容，那么可以输入：

`git log`

通过执行该命令来查看日志，如果觉得输出的信息过多，可以再加上 `--pretty=oneline` 参数，即：

`git log --pretty=oneline`

在 Git 中用 HEAD 表示当前版本，如果想回退到之前提交的版本可以使用 HEAD^，上上个版本就是 HEAD^^，往上的数目很大，比如 100，那么可以用 HEAD~100 表示。当要回退到上一版本时，输入： 

`git reset --hard HEAD^`

执行该命令后即回退到上一 commit 提交的版本，使用 `git log` 可以再看下当前版本的状态，可以看到之前最后提交的记录已没有了。如果觉得反悔了，还是想回到最后提交的版本，那么就到之前 `git log` 显示的提交信息中找到提交它生成的一串十六进制版本号的前几位，比如 051622，版本号不用写全，只要和其它已提交的版本号有区别就行了，输入：

`git reset --hard 051622`

于是 HEAD 又指向序列号为 051622 开头的那次提交。如果中途因关闭了 Git Bash 找不到想回到的提交版本号，那么可以执行：

`git reflog`

来查看已执行的命令，从输出信息中可以找到每次提交的版本号。

#### 2.2 工作区与暂存区

工作区指的是电脑中包含 .git 文件夹的目录，.git 是 Git 的版本库，Git 版本库里包含了称为 stage（或叫 index）的暂存区和 Git 为我们自动创建的第一个 master 分支，以及指向 master 的一个指针HEAD。

从之前也能看到，在把文件往 Git 版本库里添加的时候，先是用 git add 添加进去，实际上就是添加到暂存区，之后的 git commit 命令就是把暂存区的所有内容提交到当前分支。在我们创建 Git 版本库时，Git 自动为我们创建了唯一一个 master 分支，执行 `git commit` 就是在 master 分支上提交更改，可以简单理解为需要提交的文件修改通通先放到暂存区，然后一次性提交暂存区的所有修改。

#### 2.3 管理修改

对 readme.txt 进行一些操作：第一次修改 → `git add` → 第二次修改 → `git commit`，执行

`git status` 

可以看到输出的信息显示第二次修改没有提交，执行

`git diff HEAD -- readme.txt`

**可以查看工作区和版本库里面最新版的区别**，如果想提交第二次的修改，可以继续 `git add` 和 `git commit`，也可以更改之前提交的顺序，不着急提交第一次修改，先 `git add` 第二次修改，再 `git commit` 进行提交，即：第一次修改 → `git add` → 第二次修改 → `git add` → `git commit`，相当于把两次修改合并后一块提交。

#### 2.4 撤销修改

继续对 readme.txt 的文件进行修改，在还没有进行 `git add` 添加时可以执行

`git checkout -- readme.txt`

对 readme.txt 文件在工作区的修改全部撤销。`git checkout` 让工作区文件回到最近一次` git commit` 或 `git add` 时的状态。在 git for windows 版本 2.23.0 中根据 Git Bash 的提示也可以使用 `git restore <file>` 进行撤销修改。

如果对 readme.txt 文件进行修改，并执行了 `git add` 添加到暂存区，输入 `git status` 可以查看当前的状态，如果想撤销修改，可以用命令

`git reset HEAD readme.txt`

把暂存区的修改撤销掉（unstage），在 git for windows 版本 2.23.0 中 `git status` 的输出信息提示可以使用 `git restore --staged <file> ` 来撤销缓存区的修改（unstage）。

#### 2.5 删除文件

先添加一个新文件 text.txt 到 Git 并进行提交：

```
git add test.txt
git commit -m “add test.txt”
```

如果接下来你在文件管理器或者用rm命令把这个文件删了（如果删除一个非空的文件夹，可以使用 `-r` 参数）：

`rm test.txt`

输入 `git status` 命令，由于此时工作区和版本库的文件不一致，那么此时 Git 就会告诉你哪些文件被删除了。现在有两个选择，一是确实要从版本库中删除该文件，那么就用命令 `git rm` 删除掉，并且 `git commit` ：

```
git rm test.txt
git commit -m “remove test.txt”
```

执行完后 test.txt 文件就从版本库中删除了。另一种情况是删错了，但因为版本库里还有，所以可以很轻松地把误删的文件恢复到最新版本：

`git checkout -- test.txt`

`git checkout -- <file>` 其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除都可以「一键还原」。

### 3 远程仓库

首先你需要创建一个 GitHub 账号，这样可以免费获得 Git 的远程仓库，接着添加 SSH Key 来使你的 GitHub 账号和你本地的电脑进行关联，这样你每次提交代码到远程仓库时不必每次都输入 GitHub 账号密码。在 Git Bash 中输入：

`ssh-keygen -t rsa -C "your_email@example.com"`

其中 `your_email@example.com` 是你注册 GitHub 账号的邮箱，接着询问选择的保存 key 文件夹可以按 enter 键使用默认路径，然后询问设置密码时，也可以继续按 enter 键不进行密码设置，作为平常使用的话是没问题的。命令执行完后根据提示找到 .ssh 文件的路径，里面有 id_rsa 和 id_rsa.pub 两个文件，这两个就是 SSH Key 的密钥对，id_rsa 是私钥，不能泄露出去，id_rsa.pub 是公钥，可以放心告诉别人。

接着登录 GitHub 账号，进入 Settings，点击左侧的 SSH and GPG Keys，然后选择 New SSH key，接着在 Title 栏输入标识你这台电脑的名称，Key 文本框里粘贴 id_rsa.pub 文件中所有的内容，然后点击Add SSH key 进行添加。接着在命令行中输入（此处参考了 [Using Git (and GitHub) on Windows](https://www.pluralsight.com/guides/using-git-and-github-on-windows)）：

`ssh -T git@github.com`

对于询问是否进行连接，输入 yes，这是由于第一次使用 Git 连接 GitHub 时会得到一个警告，Git 使用 SSH 进行连接，而 SSH 连接在第一次验证 GitHub 服务器的 Key 时需要你确认 GitHub 的 Key 的指纹信息是否真的来自 GitHub 的服务器，如果你实在担心有人冒充 GitHub 服务器，输入 yes 前可以对照  [GitHub 的 RSA Key 的指纹信息](https://help.github.com/articles/what-are-github-s-ssh-key-fingerprints/) 是否与SSH连接给出的一致。由于之前在生成 SSH Key 我们没有设置密码，所以此时不会要求你输入密码，接着如果看到类似「Hi user! You've successfully authenticated, but GitHub does not provide shell access.」的信息，就表示远程连接成功了。

#### 3.1 添加远程库

现在在本地已创建了一个 Git 仓库，也可以在 GitHub 上创建一个 Git 仓库，并且让这两个仓库进行远程同步，这样 GitHub 上的仓库既可以作为备份，又可以让其他人通过该仓库进行协作。

首先登录你的 GitHub 账号，点击 Create a new repo 创建一个新的仓库，在 Repository name 中填入 learnGit，其它的保持默认设置，点击 Create repository 按钮就成功创建了一个新的 Git 仓库。目前 GitHub 上这个 learnGit 仓库开始空的，接着在本地的 learnGit 仓库下在 Git Bash 中运行命令：

`git remote add origin git@github.com:User/learnGit.git`

其中 `User` 是你的 GitHub 账户名，这样就把本地的 learnGit 仓库和 GitHub 上远程的 learnGit 仓库关联起来了，并且远程库的名字就是 origin，这是 Git 的默认叫法，也可以改成别的，但 origin 这个名字一看就知道是远程库。接着把本地库的所有内容推送到远程库上：

`git push -u origin master`

命令执行后把本地库的内容推送到远程，使用的 `git push` 命令，实际上是把当前分支 master 推送到远程。由于远程库是空的，第一次推送 master 分支时加上了 `-u` 参数，Git 不但会把本地的 master 分支内容推送到远程新的 master 分支，还会把本地的 master 分支和远程的 master 分支关联起来，以后再推送或者拉取时就可以简化命令。推送成功后刷新下 GitHub 中 learnGit 仓库的页面内容，可以看到远程库的内容和本地一模一样。从现在起只要本地作了提交，就可以通过命令：

`git push origin master`

把本地 master 分支的最新修改推送到 GitHub，现在你就拥有了真正的分布式版本库。

#### 3.2 从远程库克隆

现在假设从零开发，那么比较好的方式就是先创建远程库，然后从远程库克隆。首先登录 GitHub 账户创建一个新的仓库，名字叫 gitSkills，在页面中勾选 Initialize this repository with a README，这样 GitHub 会自动为我们创建一个 README.md 文件。gitSkills 仓库创建完毕后可以看到 README.md 文件。接着用 `git clone` 命令克隆出一个本地库，在 Git Bash 中用 cd 命令进入到你想保存远程 gitSkills 仓库的文件夹，接着输入：

`git clone git@github.com:User/gitSkills.git`

克隆完成后在本地的当前路径中就有一个 gitSkills 的文件夹，进入 gitSkills 可以看到该文件夹中已经有了 README.md 文件。如果多人协作开发，那么每个人各自从远程克隆一份就可以了。

你也许注意到表示 GitHub 仓库的地址不止一个，还有一个 https:// 开头的地址，实际上 Git 支持多种协议，可以用 git@ 开头的 ssh 协议，也可以用 https 等其它协议。使用 https 除了速度慢以外，还有个最大的麻烦就是每次推送都必须输入口令，但是在某些只开放 http 端口的公司内部就无法使用 ssh 协议而只能用 https。

### 4 分支管理

假设你准备开发一个新功能，有了分支你可以创建一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样既安全又不影响别人工作。**其它版本控制系统如 SVN 等都有分支管理，但用过之后你会发现这些版本控制系统创建和切换分支比蜗牛还慢，结果分支功能成了摆设，大家都不去用**。但 Git 的分支是与众不同的，无论创建、切换和删除分支都能迅速的完成。

#### 4.1 创建与合并分支

在 3.1 节版本回退里我们已经看到每次提交，Git 都把它们串成一条时间线，这条时间线就是一个分支，截止到目前只有一条时间线，在 Git 里这个分支叫主分支，即 master 分支。HEAD 严格来说不是指向提交，而是指向 master，master 才是指向提交，**所以 HEAD 指向的就是当前分支**。一开始的时候 master 分支是一条线，Git 用 master 指向最新的提交，再用 HEAD 指向 master 就能确定当前分支以及当前分支的提交点，如下图所示：

<img src="https://raw.githubusercontent.com/HaokaiMO/hello-world/master/LearningNotesofGit/create-and-combine-branches-00.JPG" width="236" height="131" alt="create-and-combine-branches-00" />

每次提交，master 分支都会向前移动一步，随着不断提交，master 分支的线也越来越长。当我们创建新的分支，如 dev 时，Git 新建了一个指针叫 dev，指向 master 相同的提交，再把 HEAD 指向 dev 就表示当前的分支在 dev 上，如下图所示：

<img src="https://raw.githubusercontent.com/HaokaiMO/hello-world/master/LearningNotesofGit/create-and-combine-branches-01.JPG" width="275" height="178" alt="create-and-combine-branches-01" />

所以 Git 创建一个分支很快，因为除了增加一个 dev 分支，改改 HEAD 的指向，工作区的文件都没有任何变化。不过从现在开始，对工作区的修改和提交就是针对 dev 分支了，比如新提交一次后，dev 指针往前移动一步，而 master 指针不变，如下图所示：

<img src="https://raw.githubusercontent.com/HaokaiMO/hello-world/master/LearningNotesofGit/create-and-combine-branches-02.JPG" width="356" height="160" alt="create-and-combine-branches-02" />

假如我们在 dev 上的工作完成了，就可以把 dev 合并到 master 上，Git 怎么合并呢？最简单的方法就是直接把 master 指向 dev 的当前提交就完成了合并，如图所示：

<img src="https://raw.githubusercontent.com/HaokaiMO/hello-world/master/LearningNotesofGit/create-and-combine-branches-03.JPG" width="277" height="157" alt="create-and-combine-branches-03" />

合并完分支后可以删除 dev 分支，删除 dev 分支就是把 dev 指针给删掉，删掉后我们就只剩一条 master 分支，如下图所示：

<img src="https://raw.githubusercontent.com/HaokaiMO/hello-world/master/LearningNotesofGit/create-and-combine-branches-04.JPG" width="305" height="129" alt="create-and-combine-branches-04" />

根据上述说明我们输入指令进行相应的练习，首先创建 dev 分支，然后切换到 dev 分支：

`git checkout -b dev`

`git checkout` 命令加上 `-b` 参数表示创建并切换，相当于以下两条命令：

```
git branch dev
git checkout dev
```

然后用 `git branch` 命令查看当前分支：

`git branch`

`git branch` 命令会列出所有的分支，在当前分支前面会标一个*号。接着在 dev 分支上正常提交，比如对 readme.txt 进行修改，修改完后进行提交：

```
git add readme.txt
git commit -m “branch test”
```

现在 dev 分支的工作完成，我们可以切换回 master 分支：

`git checkout master`

切换回 master 分支后再查看下 readme.txt 文件，可以看到刚才添加的内容不见了，因为那个修改的提交是在 dev 分支上，而 master 分支此刻的提交点并没有变，如下图所示：

<img src="https://raw.githubusercontent.com/HaokaiMO/hello-world/master/LearningNotesofGit/create-and-combine-branches-05.JPG" width="315" height="176" alt="create-and-combine-branches-05" />

现在把 dev 分支的工作成果合并到 master 分支上：

`git merge dev`

`git merge` 命令用于合并指定分支到当前分支，合并后再查看 readme.txt 内容，可以看到此时的内容和 dev 分支的最新提交是完全一样的，注意 Git Bash 上输出的 Fast-forward 信息，Git 告诉我们这次合并是「快进模式」，也就是直接把 master 指向 dev 当前提交，所以合并的速度非常快的。当然，也不是每次合并都能 Fast-forward，后面会讲到其他方式的合并。合并完成后就可以放心删除 dev 分支了：

`git branch -d dev`

删除后，用 `git branch` 查看分支，从输出结果中可以看到只剩下 master 分支了。因为创建、合并和删除分支非常快，所以 Git 鼓励你使用分支完成某个任务，合并后再删除分支，**这和直接在 master 分支上工作效果是一样的，但过程更安全**。

我们注意到切换分支使用 `git checkout <branch>`，而之前提到达到撤销修改则是 `git checkout -- <file>`，同一个命令，有两种作用，容易让人弄混。实际上在切换分支时，可以用 Git 版本提供的新的 `git switch` 命令来切换分支，创建并切换到新的 dev 分支可以使用：

`git switch -c dev`

直接切换到已有的 master 分支可以使用：

`git switch master`

使用新的 `git switch` 命令比 `git checkout` 要更容易理解。

#### 4.2 解决冲突

准备新的 feature1 分支，在新的分支下修改 readme.txt 中最后一行，然后进行提交，接着切换到 master 分支上，在 master 分支上把 readme.txt 中最后一行进行另外的修改，再提交，相关的命令如下：

```
git checkout -b feature1
git add readme.txt
git commit -m “AND simple”
git checkout master
git add readme.txt
git commit -m “& simple”
```

此时在 Git 中提交的顺序图如下所示：

<img src="https://raw.githubusercontent.com/HaokaiMO/hello-world/master/LearningNotesofGit/resolve-conflicts-00.JPG" width="313" height="209" alt="resolve-conflicts-00" />

这种情况下如果把 feature1 分支合并到 master 上就无法执行快速合并，只能试图把各自的修改合并起来，但这种合并就可能会有冲突。输入：

`git merge feature1`

命令执行后可以看到提示的文件冲突信息，并告诉我们自动合并失败必须手动解决冲突后再提交，此时输入：

`git status`

命令执行后输出的信息也会告诉我们冲突的文件。打开 readme.txt 文件，我们会看到 Git 用 `<<<<<<<`、`=======`、`>>>>>>>` 标记出不同分支的内容，手动修改此时 readme.txt 中的内容，合并成我们想要的内容，接着再进行添加和提交：

```
git add readme.txt
git commit -m “conflict fixed”
```

执行后 master 分支和 feature1 分支变成下图所示：

<img src="https://raw.githubusercontent.com/HaokaiMO/hello-world/master/LearningNotesofGit/resolve-conflicts-01.JPG" width="401" height="202" alt="resolve-conflicts-01" />

接着用 `git log --graph` 命令，执行后可以看到分支合并的情况：

`git log --graph --pretty=oneline --abbrev-commit`

最后删除 feature1 分支：

`git branch -d feature1`

此时当前的任务完成。

#### 4.3 分支管理策略

通常合并分支时，如果可能，Git 会用 Fast forward 模式，但在这种模式下删除分支后就会丢掉分支信息。如果强制禁用 Fast forward 模式，Git 就会在 merge 时生成一个新的 commit，这样从分支历史上就能看出分支信息。使用 `--no-ff` 方式的 `git merge` 相关的步骤如下：

`git checkout -b dev`

修改 readme.txt，并提交进行提交：

```
git add readme.txt
git commit -m “add merge”
```

接着切换回 master，并对 dev 分支进行合并：

```
git checkout master
git merge --no-ff -m “merge with no-ff” dev 
```

在使用 `--no-ff` 进行合并时因为要创建一个新的 commit，所以要加上 `-m` 参数把 commit 描述写进去。合并后可以用 `git log` 命令查看分支历史：

`git log --graph --pretty=oneline --abbrev-commit`

可以看到不使用 Fast forward 模式 merge 后就像这样：

<img src="https://raw.githubusercontent.com/HaokaiMO/hello-world/master/LearningNotesofGit/resolve-conflicts-02.JPG" width="347" height="192" alt="resolve-conflicts-02" />

**分支策略**：在实际开发中，我们应该按照几个基本原则进行分支管理。**首先 master 分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活**，那都在哪干活呢？干活都在 dev 分支上，也就是说 dev 分支是不稳定的，到某个时候，比如 1.0 版本发布时，再把 dev 分支合并到 master 上，在 master 分支发布 1.0 版本。你和你的小伙伴们每个人都在 dev 分支上干活，每个人都有自己的分支，时不时地往 dev 分支上合并就可以了，所以团队合作的分支看起来就像这样：

<img src="https://raw.githubusercontent.com/HaokaiMO/hello-world/master/LearningNotesofGit/resolve-conflicts-03.JPG" width="536" height="148" alt="resolve-conflicts-03" />

#### 4.4 Bug分支

当接到一个代号 101 的 bug 任务时，很自然你想创建一个 issue-101 分支来修复它，但是当前你在 dev 上进行的工作还没有提交。并不是你不想提交，而是工作只进行到一半，预计完成还需要1天时间，但是此时必须在 2 小时内修复 bug，该怎么办？Git 还提供了一个 stash 功能，可以把当前工作现场储藏起来，等以后恢复现场后继续工作：

`git stash`

执行完该命令后现在用 `git status` 查看工作区，可以看到当前的工作区是干净的，因此可以放心的创建分支来修复 bug。**首先要确定在哪个分支上修复 bug**，假定需要在 master 分支上修复，就从 master 创建临时分支：

```
git checkout master
git checkout -b issue-101
```

现在在 issue-101 分支上对 readme.txt 进行修改，然后提交：

```
git add readme.txt
git commit -m “fix bug 101”
```

修复完成后切换到 master 分支，并完成合并，最后删除 issue-101 分支：

```
git checkout master
git merge --no-ff -m ”merged bug fix 101” issue-101
git branch -d issue-101
```

这时可以回到 dev 分支继续干活，用 `git status` 命令可以看到此时工作区间是干净的，但用 `git stash list` 命令可以看到 Git 把 stash 内容存在某个地方了需要恢复一下。有两种办法进行恢复：一种是用 `git stash apply` 恢复，但恢复后 stash 内容并不删除，需要接着用 `git stash drop` 命令来删除；另一种是用 `git stash pop` 命令，恢复的同时把 stash 内容也删了：

```
git checkout dev
git status
git stash list
git stash pop
```

这时再用 `git stash list` 查看时，就看不到任何 stash 内容了。你可以多次 stash，恢复的时候先用 `git stash list` 查看，然后用命令 `git stash apply stash@{0}` 恢复指定的 stash。

在 master 分支上修复了 bug 后，由于 dev 分支时早期从 master 分支分出来的，所以这个 bug 其实在 dev 分支上也存在，那么怎么在 dev 分支上修复同样的 bug ？这时我们只需把之前 `8dc89dc fix bug 101` 的这个提交所做的修改「复制」到 dev 分支，`8dc89dc` 代表之前提交时生成的一串十六进制数开头的几位数字。为了方便操作，Git 专门提供了一个 `cherry-pick` 命令，让我们能复制一个特定的提交到当前分支：

`git cherry-pick 8dc89dc`

执行这个命令后 Git 自动给 dev 分支做了一次提交，注意这次提交的 commit 是 `c3783f9`，它不同于 master 的 `8dc89dc`，因为这两个 commit 只是改动相同，但确实是两个不同的 commit。用 `git cherry-pick` 命令我们就不需要在 dev 分支上手动再把修改 bug 的过程重复一遍。

#### 4.5 Feature分支

添加一个新功能时，你肯定不希望因为一些实验性质的代码把主分支搞乱了，所以每添加一个新功能最好在上面新建一个 feature 分支，完成后进行合并，最后再删除该 feature 分支。比如现在接到了一个开发代号为 Vulcan 的功能，于是准备开发：

`git checkout -b feature-vulcan`

之后开发完毕，准备切回 dev 分支进行合并：

```
git add vulcan.py
git status
git commit -m “add feature vulcan”
git checkout dev
```

但此时接到上级命令，因经费不足新功能必须取消，虽然是白干了但这个包含机密的分支还是必须就地销毁：

`git branch -d feature-vulcan`

执行这个命令后，Git 提示 feature-vulcan 分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除需要使用大写的 `-D` 参数，现在我们强行删除：

`git branch -D feature-vulcan`

**开发一个新的 feature 最好新建一个分支，如果要丢弃一个没有合并过的分支，就可以通过 `git branch -D <name>` 强行删除**。

#### 4.6 多人协作

从远程仓库克隆时，实际上 Git 自动把本地的 master 分支和远程的 master 分支对应起来了，并且远程仓库的默认名称是 origin，要查看远程库的信息可以用 `git remote` 或 `git remote -v` 命令来分别显示远程库的名称或名称以及抓取和推送的远程库地址，在执行 `git remoter -v` 命令时如果本地电脑没有推送的权限，那么就看不到推送（push）地址。

**推送分支就是把该分支上所有本地提交推送到远程库，推送时要指定本地分支，这样 Git 就会把该分支推送到远程库对应的远程分支上**：

`git push origin master`

如果要推送其它分支，比如 dev，那么就改成：

`git push origin dev`

但是并不是一定要把本地分支往远程推送，那么哪些分支需要推送哪些不需要呢？master 分支是主分支，因此时刻需要与远程同步；dev 分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；bug 分支只用于在本地修复 bug，没必要推到远程；feature 分支是否推送到远程取决于你是否和你的小伙伴在上面开发。

多人协作时大家会往 master 和 dev 分支上推送各自的修改。现在模拟一个你的小伙伴，可以在另一台电脑（注意要把 SSH Key 添加到 GitHub）或者同一台电脑的另一个目录下克隆：

`git clone git@github.com:User/learnGit.git`

当你的小伙伴从远程库 clone 时，默认情况下你的小伙伴只能看到本地 master 分支，不信的话可以用 `git branch` 命令查看。而此时你的小伙伴要在 dev 分支上开发，那么就必须创建远程 origin 的 dev 分支到本地，可以用下面这个命令创建本地 dev 分支：

`git checkout -b dev origin/dev`

现在他就可以在 dev 上继续修改，然后时不时的把 dev 分支 push 到远程：

```
git add env.txt
git commit -m “add env”
git push origin dev
```

你的小伙伴已经向 origin/dev 分支推送了他的提交，而碰巧你也对同样的文件做了修改并试图推送：

```
git add env.txt
git commit -m “add new env”
git push origin dev
```

此时根据命令的执行结果，输出信息会显示推送失败，解决办法也很简单，根据提示先用 git pull 把最新的提交从 origin/dev 中拉取下来，接着在本地进行合并解决冲突再推送，此时输入：

`git pull`

执行后会提示拉取失败，原因是没有指定本地 dev 分支和远程 origin/dev 分支的链接，根据提示，设置 dev 和 origin/dev 的链接，然后再进行拉取：

```
git branch --set-upstream-to=origin/dev dev
git pull
```

执行后 git pull 拉取成功，但合并有冲突需要手动解决，解决后再提交，然后 push：

```
git commit -m “fix env conflict”
git push origin dev
```

因此多人协作的工作模式通常是这样的：首先试图用 `git push origin <branch-name>` 推送自己的修改，如果推送失败则因为远程分支比你本地的更新，需要先用 `git pull` 命令进行合并，如果合并有冲突，就解决冲突在本地提交，解决冲突后或之前拉取到本地时没有发生冲突，那么再用 `git push origin <branch-name>` 推送就能成功。如果之前的 `git pull` 命令执行后提示 no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令：

`git branch --set-upstream-to=origin/<branch> dev`

进行创建。

#### 4.7 Rebase

**rebase 操作可以把本地未 push 的分叉提交历史整理成直线**。

下面举个例子，在本地的仓库的 master 分支和远程分支同步后，对 hello.py 这个文件作两次提交，用 `git log` 命令查看：

`git log --graph --pretty=oneline --abbrev-commit`

执行后 Git Bash 中显示的信息类似下图所示：

<img src="https://raw.githubusercontent.com/HaokaiMO/hello-world/master/LearningNotesofGit/rebase-00.JPG" width="401" height="240" alt="rebase-00" />

在输出信息中能看到 Git 分别用（HEAD -> master）和（origin/master）标识出当前分支的 HEAD 和远程 origin 所处的提交位置，在提交信息为 「init hello」 的远程同步后，本地进行了提交信息分别为「add comment」和「add author」的两次提交。接着尝试推送本地分支至远程：

`git push origin master`

输出的信息显示推送失败，说明有人先于我们推送了远程分支，于是先 pull 下：

`git pull`

再用 `git status` 看下状态，输出的信息显示我们本地的分支比远程超前3个提交，因此之前本地提交了两次，加上刚才 pull 下来的合并提交就是 3 个，接着用 `git log` 看下记录：

`git log --graph --pretty=oneline --abbrev-commit`

执行后 Git Bash 中显示的信息类似下图所示：

<img src="https://raw.githubusercontent.com/HaokaiMO/hello-world/master/LearningNotesofGit/rebase-01.JPG" width="657" height="221" alt="rebase-01" />

这时我们发现在本地上次和远程同步的「init hello」提交后，本地的「add comment」、「add author」、远程的「set exit=1」和本地的「Merge branch 'master' of github.com:michaelliao/learngit」这些提交记录分叉了。当然现在也可以直接把本地的分支推送到远程，但之后用 `git log` 查看提交记录时不是那么好看，此时我们可以用 rebase 命令调整下，输入：

`git rebase`

命令执行后，如果文件能自动合并，不需要手动解决冲突，那么用 `git log` 命令再查看下：

`git log --graph --pretty=oneline --abbrev-commit`

此时 Git Bash 中显示的日志信息类似下图所示：

<img src="https://raw.githubusercontent.com/HaokaiMO/hello-world/master/LearningNotesofGit/rebase-02.JPG" width="402" height="157" alt="rebase-02" />

可以看到原本分叉的提交日志记录现在变成了一条直线，我们可以发现 Git 把我们本地的「add comment」、「add author」两次提交「挪动」了位置，放到了提交信息为「set exit=1」的提交后面，rebase 操作后最终的提交内容是一致的，但我们本地的 commit 修改内容已经变化了，它的修改不再基于提交信息为「init hello」的那次提交，而是基于提交信息为「set exit=1」的提交，不过最后的「add author」提交的内容是一致的，因为它仍是在「add comment」提交之后。

如果在执行 `git rebase` 命令过程中出现冲突不能自动合并情况，根据输出的信息提示，在手动解决冲突文件后用 `git add <conflicted_files>` 或 `git rm <conflicted_files>` 进行处理，然后再使用 `git rebase --continue` 命令进行继续，在 rebase 的过程中也可以使用 `git rebase --abort` 来停止 rebase，回到 rebase 之前的状态。

最后用 push 命令把之前的本地分支推送到远程，接着使用 `git log` 命令进行查看：

```
git push origin master
git log --graph --pretty=oneline --abbrev-commit
```

命令执行后 Git Bash 中显示的信息类似下图所示：

<img src="https://raw.githubusercontent.com/HaokaiMO/hello-world/master/LearningNotesofGit/rebase-03.JPG" width="417" height="152" alt="rebase-02" />

可以看到远程分支的提交历史也是一条直线。rebase 操作可以把本地未 push 的分叉提交历史整理成直线，它的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

### 5 标签管理

发布一个版本时我们通常先在版本库中打一个标签（tag），这样就唯一确定了打标签时刻的版本，将来无论什么时候，取某个标签的版本就是把那个打标签的时刻的历史版本取出来，所以标签也是版本库的一个快照。

Git 的标签虽然是版本库的快照，但其实他就是指向某个 commit 的指针，因此创建标签和删除标签都是瞬时完成的。和每次提交生成的一串十六进制数比起来，tag 可以是一个让人容易记住的有意义的名字，它和某个 commit 绑在一起。

#### 5.1 创建标签

Git 中打标签非常简单，首先切换到需要打标签的分支上：

`git checkout master`

然后输入命令 `git tag <name>` 就可以打一个新的标签：

`git tag v1.0`

可以用 `git tag` 命令查看所有标签：

`git tag`

标签默认是打在最新提交的 commit 上，如果之前提交的版本忘记打标签了想补上怎么办？可以使用 `git log` 命令找到历史提交的 commit 序列号 id，比如之前提交的某个版本序列号开头的数字为 `f52c633`，那么要对这个提交版本进行打标签时输入：

`git tag v0.9 f52c633`

接着用 `git tag` 命令可以查看当前所有的标签，所列出的标签顺序不是按时间顺序列出的，而是按字母排序的，可以用 `git show <tagname>` 查看某一标签的信息：

`git show v0.9`

输出的信息中可以看到 v0.9 确实是打在提交的序列号 id 以 `f52c633` 开头的提交上。此外还可以创建带有说明的标签，用参数 `-a` 指定标签名，`-m` 指定说明文字：

`git tag -a v0.1 -m “version 0.1 released” 1094adb`

接着再用命令：

`git show v0.1`

就可以看到标签说明文字。

#### 5.2 操作标签

如果标签打错了也可以删除：

`git tag -d v0.1`

因为创建的标签只存储在本地，不会自动推送到远程，所以打错的标签可以在本地安全删除。如果要推送某个标签到远程，使用命令 `git push origin <tagname> ` ：

`git push origin v1.0`

或者一次性推送全部尚未推送到远程的本地标签：

`git push origin --tags`

如果标签已经推送到远程，要删除远程标签的话麻烦一点，需要先从本地删除：

`git tag -d v0.9`

然后再从远程删除，删除的命令也是 push，但格式有点区别：

`git push origin :refs/tags/v0.9`

之后可以登录 GitHub 查看是否真的从远程库删除了标签。

### 6 使用GitHub

我们通常用 GitHub 作为免费的远程仓库，如果是个人的开源项目，放到 GitHub 上是完全没问题的。其实 GitHub 还是一个开源协作社区，通过 GitHub 既可以让别人参与你的开源项目，你也可以参与别人的开源项目。

在 GitHub 上利用 Git 极其强大的克隆和分支功能，广大人民群众真正可以第一次自由参与各种开源项目了。如何参与一个开源项目呢？比如人气很高的 bootstrap 项目，这是一个非常强大的 CSS 框架，你可以访问它的 [项目主页](https://github.com/twbs/bootstrap)，之后点击 Fork 就可以在自己的账号下面克隆一个 bootstrap 仓库，然后从自己的账号下克隆：

`git clone git@github.com:User/bootstrap.git`

一定要从自己的账号下 clone 仓库，这样你才能推送修改，如果从 bootstrap 的作者仓库地址下克隆，**你将不能推送修改，因为你没有权限**。如果你想修复 bootstrap 的一个 bug 或者新增一个功能，那么可以立刻干活，干完后往自己的仓库推送，如果你希望 bootstrap 的官方库能够接受你的修改，你就可以在 GitHub 上发起一个 pull request，当然对方是否接受你的 pull request 就不一定了。

**总的来说在 GitHub 上你可以任一 Fork 开源仓库并拥有 Fork 后仓库的读写权限，之后你可以使用 pull request 给官方仓库来贡献代码**。

### 7 使用码云

在使用 GitHub 时，国内的用户经常遇到的问题是访问速度太慢，有时候还会出现无法连接的情况。如果我们希望体验 Git 飞一般的速度，可以使用国内的 Git 托管服务——码云（gitee.com）

和 GitHub 相比，码云也提供免费的 Git 仓库，此外它还集成了代码质量检测、项目演示等功能，对于团队协作开发，码云还提供了项目管理、代码托管、文档管理的服务，5 人以下小团队免费（码云的免费版本也提供私有库功能，只是有 5 个成员的上限）。

### 8 自定义Git

在前面安装 Git 后我们设置了 user.name 和 user.email，实际上 Git 还有很多可配置项，比如让 Git 显示颜色，会让命令看起来更醒目：

`git config --global color.ui true`

命令执行后，在输入和显示信息的窗口中 Git 会适当的显示不同的颜色。

#### 8.1 忽略特殊文件

有些时候你必须把某些文件放到 Git 工作目录中，但又不能提交它们，比如保存了数据库密码的配置文件等，这样每次 `git status` 命令都会显示 untracked files…，有强迫症的童鞋心里肯定不爽，好在 Git 考虑了大家的感受，在 Git 工作区的根目录下创建一个特殊的 .gitignore 文件，然后把要忽略的文件名填进去，Git 就会自动忽略这些文件。

不需要从头写.gitignore文件，GitHub已经为我们准备了 [各种配置文件](https://github.com/github/gitignore)，只需要组合一下就可以使用了，所有的配置文件可以直接在线预览。

忽略文件的原则是：

1. 忽略操作系统自动生成的文件，比如缩略图等；
2. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没有必要放进版本库，比如Java编译产生的 .class 文件；
3. 忽略你自己带有的敏感信息的配置文件，比如存放口令的配置文件；

比如你在 Windows 下进行 python 开发，那么 .gitignore 中的内容可以如下：

```
# Windows:
Thumbs.db
ehthumbs.db
Desktop.ini

# Python:
*.py[cod]
*.so
*.egg
*.egg-info
dist
build

# My configurations:
db.ini
deploy_key_rsa
```

接着把 .gitignore 文件提交到 Git 就可以了。检阅是否成功的忽略了未提交的文件，此时可以使用 `git status` 命令查看当前的工作区是否是干净的。

有时候你想添加一个文件 App.class 到 Git，但发现添加不了，原因是这个文件被 .gitignore 忽略了。如果你确实想添加该文件，可以用 `-f` 来强制添加到 Git：

`git add -f App.class`

或者你发现可能是 .gitignore 写得有些问题，需要找出来到底哪个规则写错了，可以用 `git check-ignore` 命令检查：

`git check-ignore -v App.class`

执行该命令后，输出的信息会告诉我们在 .gitignore 中是哪行规则忽略了这个文件，于是我们就可以知道应该修订哪个规则。

总体来说忽略某些文件时需要编写 .gitignore 文件，而 .gitignore 本身要放到版本库里，并且可以对 .gitignore 做版本管理。

#### 8.2 配置别名

在 Git 中可以对输入的命令用其它名字进行替换，比如使用 `git st` 表示 `git status`，要达到这样的效果需要输入：

`git config --global alias.st status`

命令执行后可以试着输入 `git st` 看看效果。还有其它的命令可以简写，比如用 co 表示 checkout、用 ci 表示 commit、用 br 表示 branch 等。在设置需要替换的命令名时 `--global` 表示的是全局参数，也就是说这些命令在这台电脑的所有 Git 仓库下都有效。

#### 8.3 配置文件

在配置 Git 时加上 `--global` 是针对当前用户起作用的，如果不加，那就只针对当前的仓库起作用。每个仓库的 Git 配置文件都放在 .git/config 文件中，用文本编辑器比如 notepad++ 打开，如果设置了命令的别名，那么在该文件中的 [alias] 后面就能看到相应的情况，如果要删除设置的命令别名，直接把对应的行删掉即可。而当前用户的 Git 配置文件默认放在用户主目录下（C:\user\username）一个隐藏的文件 .gitconfig 中。配置别名也可以直接修改这个文件，如果改错了可以删掉文件，重新通过命令配置。

给 Git 配置好别名，就可以在输入命令时偷个懒，我们鼓励这样的偷懒。

#### 8.4 搭建Git服务器

在远程仓库一节中我们讲了远程仓库实际上和本地仓库没啥不同，纯粹为了 7*24 小时开机并交换大家的修改。GitHub 就是一个免费托管开源代码的远程仓库，但是对某些视源码如生命的商业公司来说，既不想公开源码，又舍不得给 GitHub 交保护费，那就只能自己搭建一台 Git 服务器作为私有仓库使用。

搭建 Git 服务器需要准备一台运行 Linux 的机器，强烈推荐使用 Ubuntu 或 Debian，这样通过几条简单的命令就可以完成安装。

### 9 期末总结

经过了几天的学习，相信你对 Git 已经初步掌握。一开始可能觉得 Git 上手比较困难，尤其是已经熟悉 SVN 的童鞋，但没关系，多操练几次就会越用越顺手。

Git 虽然极其强大，命令繁多，但常用的就那么十来个，掌握好这十几个常用的命令，你就可以得心应手地使用 Git 了。友情附赠国外网友制作的 [Git Cheat Sheet](https://gitee.com/liaoxuefeng/learn-java/raw/master/teach/git-cheatsheet.pdf)，建议打印出来备用。

另外，英文还不错的童鞋可以经常去 [Git官网](http://git-scm.com) 看看，如果学习 Git 后，工作效率大增，有更多的空闲时间健身、看电影，那么我的教学目标也达到了。

在这里感谢廖雪峰老师制作的 [Git教程](https://www.liaoxuefeng.com/wiki/896043488029600) ，当时根据廖老师的这个教程练习下来收获不少，后来便整理了在学习这个教程过程中的一些要点内容，方便以后回顾。感兴趣的朋友也可以访问廖老师的官方网站上的 Git 教程，根据相应的例子练习学习下，要是觉得效果还不错，也可以给廖老师打个赏支持一下。

&nbsp;

&nbsp;

## 参考资料

1. [How to install and use Git on Windows](https://www.computerhope.com/issues/ch001927.htm)
2. [廖雪峰的Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)
3. [git简明指南](http://rogerdudler.github.io/git-guide/index.zh.html)
4. [Using Git (and GitHub) on Windows](https://www.pluralsight.com/guides/using-git-and-github-on-windows)

&nbsp;

&nbsp;

> **版权声明：本文采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh) 许可协议，若转载请注明出处**。