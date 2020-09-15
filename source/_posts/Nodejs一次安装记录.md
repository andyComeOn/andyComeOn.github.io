---
title: Nodejs一次安装记录
date: 2020-03-08 20:45:57
tags: Node.js
---

由于 github 上的博客太慢了，我决定在 gitee 上搭建一份博客！于疫情期间 2020-03-05 在家捯饬，想到我的 hexo 安装的比较早（大概是在 20190122，参考的文档就更早了），就打算找一个最近发布的文档参考！文档中让安装 npm install -g hexo-cli ，于是执行命令效果如下：

![执行命令效果](http://file.798run.top/img/blog/20200308/install-hexo-cli.png)

我在执行 npm install -g hexo-cli 时候在想我之前应该已经执行了类似的全局操作，这次执行会不会覆盖之前的版本？升级 hexo 依赖包不行吗？npm 升级 hexo 包的指令又是啥？带着上面的疑问我 Windows10 系统找到本机 nodejs 安装目录：C:\Program Files\nodejs，截图如下：

![nodejs安装目录](http://file.798run.top/img/blog/20200308/nodejs-install-dir.png)

看到之后发现怎么是一个快捷方式？该快捷方式地址详情如下：

![快捷方式详情](http://file.798run.top/img/blog/20200308/nodejs-shortcut.png)

怎么是 user 下 Roaming 的 nvm？nvm 是为了切换当前 nodejs 运行版本安装的一个小工具，难道是 nvm 把 nodejs 安装地址更改了？又找到我本机缓存的目录如下：

![](http://file.798run.top/img/blog/20200308/roaming.png)

hexo 之前参考的老文档是让安装 npm install hexo –g，新参考文档让安装 npm install hexo-cli –g，老文档中安装完成键入 hexo –v 显示：

![](http://file.798run.top/img/blog/20200308/hexo-v-old.png)

我在安装了 hexo-cli –g 之后也执行 hexo –v 显示：

![](http://file.798run.top/img/blog/20200308/hexo-v-new.png)

猜想是不是 hexo 的版本升级，之前安装的指令与现在安装的指令虽有点不太一样，安装完之后功能是一样的，可以在一个新电脑在尝试！

###### 思考，是不是安装了 nvm nodejs 的安装目录就是变成了快捷方式，属性值指向了 roaming 下的 nvm 了？
