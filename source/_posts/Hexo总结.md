---
title: Hexo总结
date: 2019-01-22 09:59:30
tags: 工具使用总结
---

#### 这是我个人对安装hexo的一些总结

##### 在github上新建仓库时候一定遵循下面的格式：

* ` {username} github.io `

##### 根目录下的` _config.yml `：

* 要正确的把你的新建的github的仓库地址copy到`  _config.yml `的deploy处 

##### 网上说的安装方法：

* 有的说` npm install hexo -g `（我采用）
* 有的说` npm install hexo-cli -g `
* 在你的Blog目录下执行 hexo init ,会生成很多文件，默认主题landscape
* next 主题 [theme-next](https://github.com/theme-next) [hexo-theme-next](https://github.com/theme-next/hexo-theme-next)
* 若不追求主题，在_config.yml最后几行配置好你的username.github.io仓库地址
* npm install hexo-deployer-git --save 安装git的插件
* hexo clean 、hexo g 、hexo d，这时候git 会提示你 tell me who u are，记得进入到 .deply_git目录，配置一下git 信息就行了，git config --local user.name "andyComeOn"  git config --local user.email "1174839512@qq.com"

##### 一些补充：

- 我配置的hexo就是用的默认主题landscape，没有多余设置其他，该主题默认的给分了几个板块Tags、TAG CLOUD、ARCHIVES、RECENT POSTS，
- 若是你使用 next主题，需要配置一下板块执行 hexo new page "tags" （会在你source目录下创建tagsmul）、hexo new page "categories"（会在你的source目录下创建categories目录）、hexo new page "about"，



##### 按照网上的说法安装完成之后要注意以下几点：

* 每次在根目录下执行`hexo clean` -> `hexo generate (hexo g)` -> `hexo deploy (hexo d)`
* 后来发现根本不需要执行` hexo clean `

##### hexo 怎么新建文章

* 网上说的是执行` hexo new post 'name' `,尝试可行。但是我在Hexo自己生成的hello-word.md的文档中发现指令` hexo new 'name' `少写了post。经测试亦可行！

##### hexo 怎么删除文章

* 先删除本地文件，然后通过生成和部署命令进而将远程仓库中的文件也一并删除。
* 具体来说，以最开始默认生成的hello-world.md这篇文章为例：首先进入到source / _post 文件夹中，找到hello-world.md文件，在本地直接执行删除。然后依次执行`hexo g`，`hexo d`，即可删除。

##### 发现的小问题

* 本来通过` hexo new post demo` ，生成的demo.md文件，我后来感觉命名不合理，就直接修改文件名字，发现修改之后标题没有修改过来？原因是忘记了修改了md文件中date时间![图片](https://github.githubassets.com/favicon.ico)
* 网上说的新建的md文件不能使用中文命名，经测试可行。
* 有的帖子说  npm install hexo-server --save ，安装这个啥用？我在本地测试 hexo s一样可以自动生效啊。

---

##### 参考的帖子

* [使用Hexo+Github一步步搭建属于自己的博客（基础）](https://www.cnblogs.com/fengxiongZz/p/7707219.html)、[使用Hexo+Github一步步搭建属于自己的博客（进阶）](https://www.cnblogs.com/fengxiongZz/p/7707568.html) 这2个帖子是我最开始参考的。
* [hexo+github搭建个人博客(超详细教程)](https://blog.csdn.net/ainuser/article/details/77609180) 这个帖子讲的详细，同时也讲述了安装git的一些info和使用镜像的说明。
* [2018最新版hexo+Github搭建个人博客教程（2018-1-22 更新）](https://blog.csdn.net/qq_32454537/article/details/79482908)这个是一个学生写的，写的很细。
* [2018最新版hexo+Github搭建个人博客教程（2018-1-22 更新）](https://blog.csdn.net/qq_32454537/article/details/79482908) 
* [GitHub+hexo搭建个人博客（2019新版超详细教程）](https://blog.csdn.net/weixin_42334475/article/details/101055364?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)
*  [随笔分类 - Github+Hexo建站系列](https://www.cnblogs.com/penglei-it/category/934299.html)    [Hexo+NextT基本设置【3】](https://www.cnblogs.com/penglei-it/p/hexo_next_baseset.html)
* [hexo中文文档](https://hexo.io/zh-cn/docs/index.html)  [hexo next 主题进阶设置](https://zhuanlan.zhihu.com/p/94038688)
* [研究生好博客](https://blog.newnius.com/)  [大专栏](https://www.dazhuanlan.com/life/) [半小时学会做个人博客网站](http://disixueyuan.com/index.php/article/8.html)
* [coding pages 杂谈](https://www.v2ex.com/t/548912)
* [hexo+github搭建个人博客(超详细教程)](https://blog.csdn.net/AinUser/article/details/77609180?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)



##### 需要探究的问题

* 就是我在一个电脑上，按照网上的配置在本地配了相应的环境，若我换了电脑，是否再次配置？配置好了之后，之前的历史文章怎么clone到本地，再次edit呢？

##### hexo的添加文章目录以及归档

* 在写博客过程中遇到归档的需求，搜了一篇文章说的如何归档：[Hexo站点中添加文章目录以及归档](https://www.jianshu.com/p/72408c410904)

* hexo 中landscape主题的favicon.ico不能生效，网上给出结局问题的帖子很少，很多都是废话！这个主题直接舍弃了，使用next主图吧。

* next主题出现这种错误：

  ![](http://file.798run.top/img/blog/20190122/theme-next-err.png)

* 解决办法：

![](http://file.798run.top/img/blog/20190122/fixed-err.png)


* hexo更新后的bug：[hexo生成的静态网页无法找到style.css](https://blog.csdn.net/NULL_thing/article/details/90082690)