---
title: Linux一些常用命令总结
date: 2019-01-25 10:23:45
tags: 工具使用总结
---


最近在提交git时候（我使用的是git的命令行），在执行git pull时候出现了，让编辑merge提交。一时间束手无策。决定好好看看这个命令，把常用的命令记录一下，主要参考的这篇帖子[Linux常用命令大全（非常全！！！）](https://www.cnblogs.com/yjd_hycf_space/p/7730690.html)
这篇帖子总结的也很好[linux的一些命令](https://www.jianshu.com/p/0ba3e4da38de)

---

#### 1、下面的一些常用指令我在git bash中测试，git bash只是模拟了部分的linux指令。
* clear：清除记录
* pwd：显示当前的目录
* cd ~：直接进入到用户的主目录
* cd -：返回上次所在目录
* ls：我理解的是把该目录下的所有文件列出来
* ll：我理解的是把该目录下的所有文件以列表的形式显示出来，显示的有文件创建时间，还有文件夹（drwxr-xr-x）与文件（-rw-r--r--）的区别，这个ll指令没有细查，可能是ls -l的简写，在文档上没有找到ll的指令，只是找到了指令ls -l。ll后面可以加某个目录，此时仅列举出来该目录下的文件，如 `ll .git` 会列举出来 .git 目录下文件详情！
* mkdir dir1：创建一个目录dir1 
* mkdir dir2 dir3：创建2个平级目录dir2 dir3
* mkdir -p tmp/dir1/dir2：创建一个目录树 

---

####  2、在windows的cmd下面也有一些类命令

* cd：命令进去目录
* cls：清除记录
* ls：查看文件列表 / windows 有自己特有的查看文件列表指令 dir 

---

####  3、删除和移动指令

* rm -f xx.html ：删除一个文件
* rmdir dir1：删除dir1目录，并且这个目录只能是空目录，其中不能有文件，若有文件命令行会提示 Directory not empty，感觉这个命令不用记，没什么卵用啊，一般一个目录都有文件。
* rmdir dir1 dir2：删除dir1和dir2目录
* rm -rf dir1：删除dir1目录并且把该目录下的文件内容都删除了
* rm -rf dir1 dir2：同时删除两个目录及它们的内容
* mv dir1 dir2  把dir1移动到dir2目录
* mv file1  file2 将源文件名改为目标文件名
* mv file1 dir1 将源文件移动到目标目录
* mv dir1 file1 出错
* 在 19/12/30 在弄自己双十一买的阿里云，看到帖子说

---

####  4、复制
* cp file1 dir2：●复制file1文件到dir2目录
* cp dir1/* dir2：●复制dir1下面的所有文件到dir2目录，切记是copy的dir1下所有文件，不包含dir1下的目录
* cp dir1/* . ：●复制dir1下面的所有文件到当前目录
* cp -a dir1 dir2：●复制dir1（包括dir1下边的所有目录和文件）到dir2
* cp -a dir1/* dir2：●复制dir1下边的所有目录和文件到dir2

---

####  5、新建文件
linux的touch命令不常用，一般在使用make的时候可能会用到，用来修改文件时间戳，或者新建一个不存在的文件，但是在git bash中使用touch aa.txt会直接生成文件。
* touch file1 ：创建一个新文件

---

####  6、修改、编辑文件
* 首先进到目录下，直接键入vim aa.txt 或者是vim dir1/bb.txt会出现如下截图：

![vim-edit-status](http://file.798run.top/img/blog/20190125/vim-edit-status.jpg)

按下【i, I, o, O, a, A, r, R】等任何一个字母之后才会进入编辑模式，不过大家一般都是按下i，其本意就是insert（插入）之意。按下i之后显示插入状态：

![vim-insert-status](http://file.798run.top/img/blog/20190125/vim-insert-status.jpg) 

想怎么编辑就怎能编辑。我编辑的输入的一句话：“我在进行vim编辑”。编辑之后需要按下ESC才退出编辑状态，如下截图：

![vim-esc-status](http://file.798run.top/img/blog/20190125/vim-esc-status.jpg) 

这时候选择你想要退出的模式：
* :w 保存编辑的内容，执行了这个命令指令，还是在命令行模式。
* :w! 强制写入该文件，但跟你对该文件的权限有关
* :q 离开（quit），如果修改了文件，执行这个指令会提示：已经修改但尚未保存（可用！强制执行）
* :q! 不保存修改强制离开！！这个也是很常用
* :wq 保存后离开，这个指令感觉是:w :q的简写
* :x 保存后离开
* 20190725总结，vim在xshell连接到服务器（linux）才能使用。本地的文件xshell使用vim打开，没有该指令。Git Bash vim可以使用，但是xshell的ctrl + insert（复制） 不能使用，shift + insert（粘贴）可以使用。

---

####  7、压缩解压文件
* 打包网上说了很多种有zip rar tar等很多的指令但是在git bush上我只能使用tar
* tar –cvf xx.tar file1 创建一个非压缩的xx.tar 这句话我理解创建一个非压缩的文件，意为，直接压缩的文件，压缩比很小所以叫非压缩。
* tar –cvf xx.tar file file1 file2 dir1  创建一个包含了 'file1', 'file2' 以及 'dir1'的档案文件
* tar -tf archive.tar 显示一个包中的内容 
* tar -xvf archive.tar 释放一个包
* tar -xvf archive.tar -C /tmp 将压缩包释放到 /tmp目录下 
* 其中压缩解压指令不好记忆可参考[tar参数详解](http://www.360doc.com/content/11/0620/13/2104556_128204591.shtml)   

---

#### 8、查看文件
* cat 某个文件，即在命令行显示文件内容

#### 9、一些杂记（关于linux安装目录）
* 问：Windows下软件的默认安装位置一般是c:\program files ，Linux中是否也有一个类似的目录用来存放应用软件安装后形成的文件夹？ 答：在/usr/local 下；一般的deb包（包括新立得或者apt-get下载的）都装在/usr.自己下载的压缩包或者编译的包,有些可以选择安装目录,一般放在/usr/local/,也有在/opt的。如果想知道具体位置,用命令。

* sudo是普通用户想以root的身份运行命令
* yum: 是redhat, centos 系统下的软件安装方式，基于Linux，全称为 Yellow dog Updater, Modified，是一个在Fedora和RedHat以及CentOS中的Shell前端软件包管理器基于RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包。
* rpm:  软件管理;  redhat的软件格式 rpm ：r=redhat p=package m=management 用于安装 卸载 .rpm软件

* wget 类似于迅雷，是一种下载工具，通过HTTP、HTTPS、FTP三个最常见的TCP/IP协议下载，并可以使用HTTP代理名字是World Wide Web”与“get”的结合。

* 串联下：使用wget下载一个 rpm包, 然后用 rpm -ivh  xxx.rpm  安装这个软件，嫌麻烦的话，就可以直接用  yum  install  sqoop   来自动下载和安装依赖的rpm软件。

* ap-get是ubuntu下的一个软件安装方式，它是基于debain。

* dnf是一个较新的安装命令，centos6不支持dnf，可以在centos7上安装dnf，dnf安装速度比yum要快的多，但是几乎所有的yum选项dnf都支持，并且dnf与yum共享数据库。（可理解为node中npm与yarn的状况）

* 网友回答1：yum安装，其实就是自动下载rpm软件包，然后把rpm软件包解压缩，把包里面的文件复制到相应的目录下。必须要明白，linux下的每个目录是有特殊含义的。/bin  /etc  /home /mnt  /opt等等，都是意味着这个目录下放置某种类型的文件。而你所安装的软件，是分散到这些目录里面去的。是“分散”的！你还是在windows系统里面下载一个rpm或deb的linux软件包，然后用解压软件打开看看。

* 网友回答2：如果是rpm，deb等软件包，你可以使用解压缩软件打开看看目录结构就明白了。linux的软件安装是分散在各个目录里面的，比如/usr/bin /usr/share /usr/lib 等等，总之，你要先弄明白linux的这些目录是什么意思。当然，也可以集中在一个目录里面。一些闭源的软件就集中放在/opt里面。


* 这是老东在安装lnmp的指令：
wget http://soft.vpser.net/lnmp/lnmp1.6.tar.gz -cO lnmp1.6.tar.gz && tar zxf lnmp1.6.tar.gz && cd lnmp1.6 && ./install.sh lnmp

#### 10、常用命令补充1： curl 命令
* 在shell中可以使用该命令 curl www.baidu.com 模拟在浏览器中访问，返回服务器响应。

#### 11、sz rz 下载到本地  上传到远程服务器
* sz rz 是 send Zmodem receive Zmodem的缩写，可这样记忆：都以远程服务器为中心， 那么 rz 就是指服务器收到文件，也就是上传本地文件到远程服务器。反之，sz 指远程服务器发送文件到本地。 
* 最开始我使用 rz 提示好像是没有这个指令，查资料发现要安装一个软件 `yum -y install lrzsz` 于是安装之后就可以顺利的使用 `rz`、`sz`指令了。

#### 12、ps命令
* 作用：表示process show，查看进程
* 语法：# ps -ef
* 选项含义：-e：等价于-A，all，表示全部  -f：表示full，显示全部的列

    ![ps_eg.png](http://file.798run.top/img/blog/20190125/ps_eg.png)
    UID：该进程的启动用户名；
    PID：process id，进程的id号
    PPID：parent process id，父级进程id号
    C：表示的cpu的使用情况
    STIME：start time，启动时间
    TTY：终端的设备编号，“？”表示该进程不是由终端发起的
    TIME：持续运行的时间
    CMD：command，显示进程的名称或者位置

* 补充：结束进程的指令 # kill PID

#### 13、在linux中新建文件
* 我在自己的xshell中新建一个aa.txt 并且编辑文件添加一些数字文本：123478787。下载到windows本地，发现编码是ANSI。但是我针对新建的文件添加文本中包含中文字符，该文件下载到本地之后，发现编码就是UTF-8。猜想这是linux自动转换格式的原因!

#### 14、配置相关（网上帖子）
* 系统配置文件基本都放在/etc下
* 个人配置文件放在用户目录下/usr
* 这些默认配置，我使用的 lnmp 安装（个人理解：一部分软件是按照 lnmp 的默认配置安装，还有一些 linux的默认安装）可以查看帖子 [[LNMP添加、删除虚拟主机及伪静态使用教程](https://lnmp.org/faq/lnmp-vhost-add-howto.html)](https://lnmp.org/faq/lnmp-vhost-add-howto.html)

#### 15、rpm -qa 查看 linux 安装的软件

* rpm -qa | grep wegt 单独查看是否安装 wegt。应为 rpm -qa 会打印所有安装软件，不容易查看，我们可以使用此命令过滤某软件，查看是否安装之！

#### 16、linux 如何查看是否开启、重启某进程如：nginx mysql
* service nginx status 
* service mysql status
* service mysql restart 
* service nginx restart

#### 17、Windows 查看端口
* netstat -a -n 出现列表 分别指：协议 本地地址 外部地址 状态
* 状态listen：表示FTP服务启动后首先处于侦听（LISTENING）状态？？？
* 状态establish：ESTABLISHED的意思是建立连接，表示两台机器正在通信
* CLOSE_WAIT表示对方主动关闭连接或者网络异常导致连接中断，此时我方要调用close()来使得连接正确关闭。
* TIME_WAIT表示我方主动调用close()断开连接，收到对方确认后状态变为TIME_WAIT。
* 如果7626端口显示为LISTENING（正在监听等待连接）状态，电脑极有可能感染了病毒！！！

#### 18、linux下如何查看某软件是否已安装
* rpm包安装的，可以用rpm -qa看到 `  rpm -qa | grep php `
* 以deb包安装的dpkg -l能看到 `  dpkg -l| grep php `
* yum方法安装的，可以用yum list installed查找 ` yum list installed | grep php `
* 源码包安装，可以通过文件查找 ` find  / -name php `
