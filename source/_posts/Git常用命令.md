---
title: Git常用命令
date: 2019-11-25 17:11:19
tags: Git相关
---

> 基本的指令每天都在使用，可是新建分支，合并分支使用的相对少一些！现记录一下使用过程中遇到的问题！

### 1、ums 仓库

- 默认已创建 master 分支，部署项目（为了给邱双弄 Nginx 接口代理），于是我 `git pull` 截图如下：

![ums-git-pull](http://file.798run.top/img/blog/20200717/ums-git-pull.png)

图中看到新增加了 dev 分支。于是我就执行 `git checkout -b dev` 切换到 dev 分支，随即 `git pull`，提示如下：

![checkout-pull](http://file.798run.top/img/blog/20200717/checkout-pull.png)

已经不止一次看到上面的截图了！心想先不管它，先干活。我就在 dev 分支上修改了一个文件，于是 `git status`、`git add .`、`git commit -m "修改 .gitignore 文件"`、`git pull`、`git push`。push 的时候出现提示如下：

![git-push](http://file.798run.top/img/blog/20200717/git-push.png)

push 没成功看开我必须要解决提示的问题，于是网上搜，参考：[git pull 报错 There is no tracking information for the current branch...](https://www.jianshu.com/p/f6f7dae0b8b6)。按照文中提示，我执行 `git branch --set-upstream-to=origin/dev dev`。随后又通过 `git branch -vv` 确认是否关联。总的截图如下：

![set-upstream-vv](http://file.798run.top/img/blog/20200717/set-upstream-vv.png)

上图则说明关联成功，git pull 拉下来很多代码！随后我又重复 `git add .`、`git commit -m "修改 .gitignore 文件"`、`git pull`、`git push`。把我刚才修改的文件 push 到 dev 了！

- 20200808 补记：在看方的 express 章节时候，看到其使用 git 执行如下：创建一个本地目录 express-demo-3，随后 git init 则生成一个 .git 目录。并在他的 github 上新建了 express-demo-3 仓库。随后执行 `git add .`、`git commit -m init` 、 `git remote add orgin git@github.com:FrankFang/express-demo-3.git`、`git push -u orgin master`。注：最后两个指令是在你初始化了仓库之后，github 上生成的！最后就 ok 了！并没有遇到我之前的提示啥 upstream 之类的报错，这说明这也是一种方法！但是我又想到我在秋果的 git 仓库是不是没有配置啥钩子才出现这种错误，还是说 github、gitee 等已经给你创建了啥钩子呢？

* 20200817 在家弄 peter 分配的 CRM 系统，说让参考墨宇的 fms，其项目一直跑不起来有问题：他用 Mac 在 git 上提交了 import '@/keren /keren.vue'（不知道为啥竟然能提交，在 Windows 上 vscode 直接回提示你目录不能这样创建）。周一，墨宇说的让删除仓库。重新 init 一个仓库！我初始化 fms.git 仓库之后：在本地新建 a.txt。开始 git add . 之一些列的操作，最后 git pull 出现如下截图：

![git-pull](http://file.798run.top/img/blog/20200818/git-pull.png)

看到这我还以为是 git 又报啥错了。于是就执行操作如下：

![git-pull](http://file.798run.top/img/blog/20200818/git-upstream.png)

搜了一堆帖子，发现不行。后来查看自己写的 git 总结文档，发现在新建 dev 分支时候，才需要执行 git branch --set-upstream-to=origin/dev dev
于是我就直接 push 了，没啥报错！上的提示是你第一次 pull 默认的提示。

### 2、查看、切换分支

- 查看分支是 `git branch` 后面可以加参数 -a （指的是所有的分支）

- 切换分支 `git checkout 分支名`

- 创建分支 `git checkout -b dev`，此指令创建一个 dev 分支，若是 dev 分支已经存在，命令行会有提示：dev has already exists！

- 创建于切换其实是一个指令，仅仅是添加的参数不同！
