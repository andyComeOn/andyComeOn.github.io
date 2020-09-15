---
title: Git在linux上的一次使用记录
date: 2020-03-16 14:42:49
tags: Git相关
---

## 记录一次Git 在 linux的操作过程

##### 1、linux主机是百度云活动免费试用3个月，是CentOS 。

* 系统默认安装的有 git 客户端，键入 git --version 显示 1.8.3.1。我想试试如何在 linux 下试用 git 拉取代码。由于之前我弄过很多次，直接copy 我电脑中的 .ssh\id_rsa | id_rsa.pub 就能用，于是我直接把我ddyg工作的ThinkPad中的id_rsa | id_rsa.pub 移动到 linux ~/.ssh 目录中，我直接执行 git clone git@github.com:andyComeOn/CXADView.git  提示如下：

![know_hosts](http://file.798run.top/img/blog/20200316/know_hosts.jpg)

我想是不是 linux 不能直接 copy id_rsa.pub文件，于是我想干脆自己生成一对新的 id_rsa 得了。于是在linux 中依次执行如下：

![get_ssh_key](http://file.798run.top/img/blog/20200316/get_ssh_key.jpg)

再次 clone 依旧是不成功，于是在网上开始找答案，让修改 /etc/ssh/ssh_config文件

![ssh_config](http://file.798run.top/img/blog/20200316/ssh_config.jpg)

让把啥 ask 改成 no，依旧是不行，后来还让在 ~/.ssh/ 创建 config 文件加上 LogLeval=什么我忘记了，尝试如下

![can_not_remote](http://file.798run.top/img/blog/20200316/can_not_remote.jpg)

依旧不行，后来想到我在自己电脑上配置的多个git拉取代码，不就是在config 配置的吗？于是我直接把自己的配置的代码修改之后放到我的~/.ssh/config中，代码如下：

~~~
Host github.com
HostName github.com
User andyComeOn  
IdentityFile ~/.ssh/id_rsa_baiduyun
PreferredAuthentications publickey
IdentitiesOnly yes
~~~

问题解决，网上说也不一定对，自己要摸索！

补记：我自己想着能不能把我的 gitee 的公钥也配置一下，直接 copy 电脑的 id_rsa_gitee_117 文件，不行，linux下不让这样搞，只能再次生产 ssh-keygen -t rsa -C "1174839512@qq.com" ，配置好config 就可以拉取 gitee 仓库的代码了。

##### 2、记录一下阿里云服务器 git 拉取gitee项目过程

* 在20200317这天想把自己本地学习 laravel 项目部署到宝塔上，已经知道 bt 可以自动创建站点，心想自己的项目使用 git 拉取更方便，于是就在自己的阿里云服务器上配置 ssh key，由于服务器默认自带 git ，我就直接执行：` git config --local user.name "andyComeOn" ` 效果：
![](http://file.798run.top/img/blog/20200316/could_not_lock_config.jpg)
后来发现这是我马虎，键入的指令错了，正确的应该是：` git config --global user.name "andyComeOn" `，键入之后会在 ~/目录下生成一个 `.gitconfig` 文件，接着说上面的截图错误，如果你在某个目录执行 `git config --local user.name "andyComeOn" ` 必须是该目录下有一个 `.git` 目录 。
