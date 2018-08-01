#### 目标
* git对项目开发和管理非常重要
* 实际操作中遇到很多问题
* 所以想通过廖雪峰的教程系统的学习一下

#### 参考
[廖雪峰git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

#### Git简介
* Git是目前世界上最先进的分布式版本控制系统。能自动记录每次文件的改动，还可以让同事协作编辑，方便管理。
* 2008年，GitHub网站上线，它为开源项目免费提供Git存储，无数开源项目开始迁移至GitHub

#### 安装Git
##### Windows上安装Git
在Windows上使用Git，可以从[Git官网](https://git-scm.com/downloads)直接下载安装程序，（网速慢的同学请移步[国内镜像](https://pan.baidu.com/s/1kU5OCOB#list/path=%2Fpub%2Fgit)），然后按默认选项安装即可。

安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功！

安装完成后，还需要最后一步设置，在命令行输入：
```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```
#### 创建版本库
* 版本库又名仓库，英文名repository。你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。
* 创建方法
第一步，选择一个合适的地方，创建一个空目录：
```
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit
```
第二步，通过`git init`命令把这个目录变成Git可以管理的仓库：
```
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```
* 添加文件到Git仓库：

  1. 使用命令`git add <file>`，注意，可反复多次使用，添加多个文件；
  2. 使用命令`git commit -m <message>`，完成。
  3. 注意：每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中。

#### (单机)时光机穿梭

* 要随时掌握工作区的状态，使用`git status`命令。
* 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。
* `git log`可以查看版本修改的历史记录，显示从最近到最远的提交日志
* HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
* 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
* 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。
* 工作区和暂存区：工作区是目录中的文件；暂存区保留修改的部分，前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：
  1. 第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；
  2. 第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。
* 撤销修改
  1. 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

  2. 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

  3. 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考`版本回退`一节，不过前提是没有推送到远程库。
* 删除文件
  * `git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。
  * 命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。

#### 远程仓库
* 在这个世界上有个叫GitHub的神奇的网站，从名字就可以看出，这个网站就是提供Git仓库托管服务的，所以，只要注册一个GitHub账号，就可以免费获得Git远程仓库。
* 由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：
  1. 创建SSH Key。在用户主目录下，看看有没有`.ssh`目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
```
$ ssh-keygen -t rsa -C "youremail@example.com"
```
  你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。
  如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。
  2. 登陆GitHub，打开“Account settings”，“SSH Keys”页面;
  然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容;
* 为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。
* 在GitHub上免费托管的Git仓库，任何人都可以看到喔（但只有你自己才能改）。所以，不要把敏感信息放进去。
* 如果你不想让别人看到Git库，有两个办法，一个是交点保护费，让GitHub把公开的仓库变成私有的，这样别人就看不见了（不可读更不可写）。另一个办法是自己动手，搭一个Git服务器，因为是你自己的Git服务器，所以别人也是看不见的。这个方法我们后面会讲到的，相当简单，公司内部开发必备。

##### 添加远程库（先有本地库后有远程库，需要关联远程库的情况）
* 要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；
如：
```
$ git remote add origin git@github.com:michaelliao/learngit.git
```
* 关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；
* 此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

##### 从远程库克隆（先创建远程库后从远程库克隆）
1. 登陆GitHub，创建一个新的仓库，名字叫`gitskills`。勾选`Initialize this repository with a README`，这样GitHub会自动为我们创建一个`README.md`文件。创建完毕后，可以看到`README.md`文件
2. 现在，远程库已经准备好了，下一步是用命令`git clone`克隆一个本地库(注意将git仓库换成你自己的)：
```
$ git clone git@github.com:michaelliao/gitskills.git
Cloning into 'gitskills'...
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 3
Receiving objects: 100% (3/3), done.
```
3. 要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。
Git支持多种协议，包括`https`，但通过ssh支持的原生git协议速度最快。
#### 分支管理
* 因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在master分支上工作效果是一样的，但过程更安全。
* 常用命令：

  查看分支：`git branch`

  创建分支：`git branch <name>`

  切换分支：`git checkout <name>`

  创建+切换分支：`git checkout -b <name>`

  合并某分支到当前分支：`git merge <name>`

  删除分支：`git branch -d <name>`
* 解决冲突：如果分支和master分支同时有修改内容，需要先解决冲突(只修改master分支，删除掉分支)。解决冲突后再提交，合并完成。解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。用`git log --graph`命令可以看到分支合并图。
* 分支策略：在实际开发中，我们应该按照几个基本原则进行分支管理：
  * 首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
  * 那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；
  * 你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了
  * 合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。
