---
title: Shell总结
date: 2019-01-23 13:12:37
tags:
---

由于开发人员多多少少会给shell打交道，自己总结一下shell的相关文档

#### 1、界面

大家在使用windows电脑时候，多多少少都会给shell打交道，我们进行的`cmd`命令、以及windows自带的

![Windows-PowerShell](//39.106.63.190/img/blog/20190123/Windows-PowerShell0.png)

打开的界面如下：
![Windows-PowerShell-GUI](//39.106.63.190/img/blog/20190123/Windows-PowerShell1.png)
还有大家常用的
![ConEum](//39.106.63.190/img/blog/20190123/ConEum.png)
ConEum终端相比windows自带增加了一些键盘复制粘贴等常用功能；还有大家在windows上安装的git，它自带也有一个git bush（它相比前面介绍的终端增加了很多linux命令行指令）。

#### 2、远程shell设置记录

最近买了一个腾讯云的服务器，打算使用shell来远程控制服务器，在腾讯云的文档上介绍了2种方式，首先介绍密码登录` Linux `实例

* 使用远程登录软件 ，采用密码登录` Linux `实例，它选择使用`PuTTY`做了介绍。

* 登录`PuTTY`的[官网](https://www.putty.org/)，页面如下：
![PuTTY](http://file.798run.top/img/blog/20190123/PuTTY-org.jpg)
在网上搜了一下，大致的意思是这里有3种的下载版本，第一种是最官方的、第二种是在官方安装包的基础上（二次开发增加了实用的功能）、第三种不是很清楚。我是直接下载的第一种安装包。很快安装完成，之后弹出来一个`README.txt`，由于在桌面上没找到快捷方式，感觉很懵。只好硬着头皮看`README.txt`，该文件告诉我们安装目录在`C:\Program Files\PuTTY`，该目录截图如下：
![PuTTY-dir](http://file.798run.top/img/blog/20190123/PuTTY-dir.jpg)
于是明白PuTTY的很多功能入口在该目录！

* 接着说腾讯云的操作文档（采用密码登录` Linux `实例），在刚才的目录下找到`putty.exe`文件打开是一个`GUI`， 如图：
![PuTTY-Configuration](http://file.798run.top/img/blog/20190123/PuTTY-Configuration.jpg)
按照提示输入你自己买的服务器地址，最后点击Open按妞。弹出GUI如下：
![PuTTY-exe-GUI](http://file.798run.top/img/blog/20190123/PuTTY-exe-GUI.jpg)
这时候输入密码（注输入密码时候不显示小黑点一定要小心）就连接上了，默认是根目录。

* 上面介绍了使用账号密码的登录方式，但是这种方式有一个不友好的操作，就是每次都要重新输入密码，这对于开发人员来说很麻烦。怎么解决？这时候就有了SSH的免密的配置。

#### 3、 使用远程登录软件采用 SSH 密钥登录。

* ssh 是什么？ 这是20190923补记。 ssh（secure shell，安全外壳协议）。该协议有2个常用的作用：远程连接、远程文件传输。协议使用端口号：默认是22。但是该端口号是可以被修改的，如果需要修改，则需要修改ssh服务的配置文件如下：
![ssh_config](http://file.798run.top/img/blog/20190123/ssh_config.png);

* 采用SSH 密钥登录，首先是在目录`C:\Program Files\PuTTY`中找到`puttygen.exe`，双击打开如图：
![puttygen-GUI](http://file.798run.top/img/blog/20190123/puttygen-GUI.jpg)
点击`Generate`，效果如下：
![puttygen-GUI-Generate](http://file.798run.top/img/blog/20190123/puttygen-GUI-Generate.jpg)
完成之后点击`Save private key`，则会把文件保存在本地（我在保存时候，想保存到`C:\Program Files\PuTTY`下）但是好像提示什么需要管理员权限，默认给我保存到了`C:\Users\user`。

* 再次回到
![PuTTY-Configuration-noEdit](http://file.798run.top/img/blog/20190123/PuTTY-Configuration-noEdit.jpg)
找到`Connection-SSH-Auth`
![Connection-SSH-Auth](http://file.798run.top/img/blog/20190123/Connection-SSH-Auth.jpg)
点击Browser，找到你的刚才存的ppk文件，再回到
![Connection-SSH-Auth](http://file.798run.top/img/blog/20190123/Connection-SSH-Auth.jpg)
Session项输入你的服务器地址。