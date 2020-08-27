# Linux

> 需要掌握：
>
> Linux背景
>
> 系统操作：
>
> 服务管理：网络管理、软件包安装、进程管理、内存管理以及磁盘管理
>
> Shell脚本
>
> 文本操作
>
> 常见服务搭建——DNS，HTTP等，家庭NAS搭建共享存储

参考：极客时间-Linux实战技能100讲



## 1. 背景介绍













## 2. Linux文件权限与目录配置

环境准备

- 云主机
- 无数据的PC（不推荐多系统混跑）
- 虚拟机（推荐方式）



Linux的常用版本

- 内核版本（https://www.kernel.org/）
  - 主版本号
  - 次版本号（奇数为开发版，偶数为稳定版）
  - 末版本号

- 发行版本
  - RedHat Enterprise Linux
  - Fedora
  - CentOS（基于RedHat编译而来）
  - Debian
  - Ubuntu



CentOS下载地址：http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-DVD-1810.iso

http://isoredirect.centos.org/centos/7/isos/x86_64/

http://mirror1.es.uci.edu/centos/7.6.1810/isos/x86_64/



root用户登录或者自建用户登录

点击未列出即可使用root用户登录



```
# 切换到字符界面
init 3

localhost login: 

# 切换用户
exit

root
password


```



终端的使用

- 图形终端
- 命令行终端
- 远程终端（SSH、VNC）



常见目录结构

```
/ 						根目录
/root 					root用户的家目录
/home/username			普通用户目录
/etc					配置文件目录（类似Windows里面的注册表）
/bin					命令目录
/sbin					管理命令目录
/usr/bin /usr/sbin		系统预装的其他命令
```



```
ls /
ls /root
ls /bin
ls /sbin
```



万能的帮助命令

- man	帮助
- help帮助
- info帮助

- 使用网络资源（搜索引擎和官方文档）

```
# man是manual的缩写
man ls
man man
man 7 man # 获取第一章shell的帮助内容
man 1 passwd # 获取命令行的帮助
man 5 passwd # 获取配置文件的帮助命令
man -a passwd

# help，shell(解释器)自带的命令成为内部命令，其他的都是外部命令
# 内部命令使用help帮助
help cd
# 外部命令使用help帮助
ls --help

# 使用type来查看命令的类型
type cd
type ls

# info帮助比help更加详细，作为help的补充
info ls
```



为什么要学习帮助命令？

- Linux的基本操作就是命令行
- 海量的命令不适合“死记硬背”



### pwd、ls和cd命令

> 文件查看、目录文件创建与删除、通配符、文件操作和文本内容查看。

```
# pwd			显示当前的目录名称
pwd
man pwd

# cd			更改当前的操作目录
cd /path/to/...
cd ./path/to/...
cd ../path/to/...

# ls			查看当前目录下的文件
# ls [选项, 选项...] 参数...
ls -l # 长格式显示文件
ls -a # 显示隐藏文件
ls -r # 逆序显示
ls -t # 按照时间顺序显示
ls -R # 递归显示
```



/ 和 /root 的区别？

/ 是根目录

/root 是 root用户的家目录



普通用户切换root用户

```bash
su - root  # 需要输入密码
```



```bash
# 单个目录
ls  # 不接文件夹就是查看当前目录
ls /root

# 多个目录
ls /root /

ls -l
[root@CentOS7_1 ~]# ls -l
total 40
-rw-------. 1 root root 3090 Aug 16 14:51 anaconda-ks.cfg
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Desktop
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Documents
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Downloads
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Music
-rw-------. 1 root root 2374 Aug 16 14:51 original-ks.cfg
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Pictures
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Public
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Templates
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Videos

# 第一个代表文件类型，d代表文件夹，-代表普通文件

[root@CentOS7_1 ~]# ls -l -a
total 92
dr-xr-x---. 15 root root 4096 Aug 16 14:52 .
dr-xr-xr-x. 18 root root 4096 Aug 16 14:49 ..
-rw-------.  1 root root 3090 Aug 16 14:51 anaconda-ks.cfg
-rw-r--r--.  1 root root   18 Dec 29  2013 .bash_logout
-rw-r--r--.  1 root root  176 Dec 29  2013 .bash_profile
-rw-r--r--.  1 root root  176 Dec 29  2013 .bashrc
drwx------. 12 root root 4096 Aug 16 14:52 .cache
drwxr-xr-x. 13 root root 4096 Aug 16 14:52 .config
-rw-r--r--.  1 root root  100 Dec 29  2013 .cshrc
drwx------.  3 root root 4096 Aug 16 14:52 .dbus
drwxr-xr-x.  2 root root 4096 Aug 16 14:52 Desktop
drwxr-xr-x.  2 root root 4096 Aug 16 14:52 Documents
drwxr-xr-x.  2 root root 4096 Aug 16 14:52 Downloads
-rw-------.  1 root root  314 Aug 16 14:52 .ICEauthority
drwx------.  3 root root 4096 Aug 16 14:52 .local
drwxr-xr-x.  2 root root 4096 Aug 16 14:52 Music
-rw-------.  1 root root 2374 Aug 16 14:51 original-ks.cfg
drwxr-xr-x.  2 root root 4096 Aug 16 14:52 .parallels
drwxr-xr-x.  2 root root 4096 Aug 16 14:52 Pictures
drwxr-xr-x.  2 root root 4096 Aug 16 14:52 Public
-rw-r--r--.  1 root root  129 Dec 29  2013 .tcshrc
drwxr-xr-x.  2 root root 4096 Aug 16 14:52 Templates
drwxr-xr-x.  2 root root 4096 Aug 16 14:52 Videos


[root@CentOS7_1 ~]# ls -l -r
total 40
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Videos
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Templates
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Public
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Pictures
-rw-------. 1 root root 2374 Aug 16 14:51 original-ks.cfg
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Music
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Downloads
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Documents
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Desktop
-rw-------. 1 root root 3090 Aug 16 14:51 anaconda-ks.cfg


[root@CentOS7_1 ~]# ls -l -r -t
total 40
-rw-------. 1 root root 2374 Aug 16 14:51 original-ks.cfg
-rw-------. 1 root root 3090 Aug 16 14:51 anaconda-ks.cfg
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Videos
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Templates
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Public
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Pictures
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Music
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Downloads
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Documents
drwxr-xr-x. 2 root root 4096 Aug 16 14:52 Desktop

ls -lrt


```



```bash
[root@CentOS7_1 ~]# ls /etc/sysconfig/network-scripts/
ifcfg-eth0   ifdown-ppp       ifup-ib      ifup-Team
ifcfg-lo     ifdown-routes    ifup-ippp    ifup-TeamPort
ifdown       ifdown-sit       ifup-ipv6    ifup-tunnel
ifdown-bnep  ifdown-Team      ifup-isdn    ifup-wireless
ifdown-eth   ifdown-TeamPort  ifup-plip    init.ipv6-global
ifdown-ib    ifdown-tunnel    ifup-plusb   network-functions
ifdown-ippp  ifup             ifup-post    network-functions-ipv6
ifdown-ipv6  ifup-aliases     ifup-ppp
ifdown-isdn  ifup-bnep        ifup-routes
ifdown-post  ifup-eth         ifup-sit
```



```bash
# cd			更改当前的操作目录
cd /path/to/...
cd ./path/to/...
cd ../path/to/...

cd /etc/sysconfig/network-scripts/
cd /root

cd -  # 返回之前切换过的目录

cd ..  # 返回上一级目录
```



### 创建和删除目录

```bash
# 创建一个空的目录
mkdir test

# 创建多个文件夹
mkdir test1 test2 test3

# 
mkdir /a/b/c

# 创建多级文件夹
mkdir -p /a/b/c/d/e/f/g
ls /a
ls -R /a

# 删除文件夹，只能删除空文件夹
rmdir a

# 可以删除非空文件夹
rm -rf a
```





### 复制和移动文件

常用参数

- -r 复制目录

- -p 保留用户、权限、时间等文件属性

- -a 等同于-dpR

```bash
# copy复制文件，不能操作目录
cp /root/a /tmp

# 
cp -r /root/a /tmp

touch /filea
cp /filea /tmp

# 保留原有的信心，时间等
cp -p 

# 保留所有信息，权限，属主等
cp -a


# 重命名
mv 

# 移动改名一起操作
mv /tmp/fileb /filec

# 如果操作的是目录的话，需要加上-r


# 移动当前目录下的所有问价
mv * test
```





复制目录的话，需要加上 -r

```bash
cp -r /root/a /tmp
```



复制文件

```bash
cp /filea /tmp
```



复制需要带权限的话，加上-a或者-p





改名

```bash
mv /filea /fileb
```





移动和改名同时

```bash
mv /tmp/filea /filec
```





通配符的使用

```bash
*
mv file* /

?
cp file? /
```





### 文本查看命令

```bash
# cat	文本内容显示到终端
cat /tmp/demo

# head	查看文件开头
head /tmp/demo
head -5 /tmp/demo

# tail	查看文件结尾
tail /tmp/demo
tail -3 /tmp/demo
tail -f /tmp/demo # 实时更新文件的变化

# wc	统计文件内容信息
wc -l /tmp/demo
more /tmp/demo
less /tmp/demo
```









### 打包与压缩

- 打包

最早的 Linux 备份介质是磁带，使用的命令是 tar

可以打包后的磁带文件进行压缩存储，压缩的命令是 gzip 和 bzip2

经常使用的扩展名是 .tar.gz, .tar.bz2, .tgz



对 Linux 备份主要是备份/etc文件夹

```bash
tar cf /tmp/etc-backup.tar /etc

# 查看打包后文件的大小
ls -lh /tmp/etc-backup.tar

[root@centos7_1 ~]# ls -lh /tmp/etc-backup.tar*
-rw-r--r--. 1 root root 37M 8月  19 21:03 /tmp/etc-backup.tar
-rw-r--r--. 1 root root 11M 8月  19 21:06 /tmp/etc-backup.tar.bz2
-rw-r--r--. 1 root root 12M 8月  19 21:05 /tmp/etc-backup.tar.gz

```



tar 打包命令

常用参数

- c 打包
- x 解包
- f 指定操作类型为文件



```bash
# 解压缩
tar xf /tmp/etc-backup.tar -C /root
```

经常用来备份文件





### Vim的四种模式

强大的文本编辑器 vi

多模式产生的原因：

- 正常模式（Normal-mode）

大写的I

i；I

a；A

o下一行插入；O上一行插入



h向左移动，l向右移动



复制，剪切，粘贴

单行复制yy，屏幕上没有任何显示，p键来粘贴

复制多行，比如3yy就是复制三行

y+$：复制从光标位置到行末尾的文本，p键粘贴

剪切：

dd：剪切单行

d$：剪切从光标位置到行末尾的文本，p键粘贴



撤销的指令

u：撤销

ctrl+r键：重做



删除命令

x：删除光标位置的单个字符

先按r键，然后按想要更换的字符即可完成更换。



:set nu显示行号



11+G

g：移动到第一行

G：移动到最后一行

$：移动到单行的末尾

^：移动到单行的行头



保存文件

```bash
:w /root/a.txt

:wq
```









- 插入模式（Insert-mode）





- 命令模式（Command-mode）

```
# 使用命令行
:!ifconfig

# 查看x字符，如果存在多个x，按n来查看下一个，N来查找上一个
/x

# 替换
:s/old/new
:%s/old/new
:%s/old/new/g # g 代表全局操作
:3,5s/old/new
```





vim的配置文件

```bash
vim /etc/vimrc
```











- 可视模式（Visual-mode）

三种进入可视模式的方法

v				字符可视模式

V				行可视模式

ctrl+v		块可视模式















### 用户和权限管理

```bash
# 只有root才权限创建新的用户
useradd

userdel -r # 加上-r选项后用户的家目录就不会被保存
userdel wilson
ls /home/

passwd # 更改当前用户的密码
passwd wilson # 更改wilson用户的密码

# 修改一个用户账号的信息
usermod

# 用户的生命周期
chage
```



```
id root
id wilson
id paul
```



```bash
# 从root童虎切换到user1用户
su - user1 # 中间的减号代表环境切换到user1的环境
```



su和sudo

```bash
# su			切换用户
# su - username 使用 login shell 方式切换用户

# sudo			以其他用户身份执行命令
# visudo		设置需要使用sudo的用户（组）
```

普通用户是没有办法进行 `shutdown` 操作的，只有 root 用户可以。

```bash
visudo

# 在最后一行添加
user3 ALL=/sbin/shutdown -c

su - root
shutdown -h 30
su - user3
shutdown -c # 此处是错误的
sudo /sbin/shutdown -c
```













用户和用户组配置文件介绍

/etc

/etc/shadow

/etc/group



```bash
paul:x:1000:1000:paul:/home/paul:/bin/bash
```





学习到22课





### su和sudo















### 查看文件权限的方法





![image-20200820093559362](https://tva1.sinaimg.cn/large/007S8ZIlly1ghx0zkwi4oj311u08uq69.jpg)



权限对于文件的重要意义







权限对于目录的重要意义

针对目录，r，w，x分别是什么意义？





| 组件 | 内容         | r            | w            | x                |
| ---- | ------------ | ------------ | ------------ | ---------------- |
| 文件 | 详细资料data | 读取文件内容 | 修改文件内容 | 执行文件内容     |
| 目录 | 文档名       | 读取文档名   | 修改文档名   | 进入该目录的权限 |

**能不能进入某一个目录，只与该目录的 x 权限有关！（重要）**

很多朋友在架设网站的时候都会卡在一些权限的设定上，他们开放目录数据给因特网的任何人来浏览，却只开放 r 的权限，如上面的范例所示那样，那样的结果就是导致网站服务器软件无法到该目录下读取文件(最多只能看到文件名)， 最终用户总是无法正确的查阅到文件的内容(显示权限不足啊！)。要注意：要开放目录给任何人浏览时，应该至少也要给予 r 及 x 的权限，但 w 权限不可随便给！



类型

```bash
-	普通文件

d	目录文件

# 一般是外接设备，比如移动硬盘
b	块特殊文件

c	字符特殊文件

# 其实就是快捷方式
l	符号链接

f	命名管道

s	套接字文件
```



普通文件权限的表达方式

```bash
# 字符权限的表达方法
r	读
w	写
x	执行

# 数字权限的表示方法
r = 4
w = 2
x = 1
```



举个例子

```bash
-rw-r-xr-- 1 username groupname mtime filename

这是一个普通文件
rw-	文件属主的权限
r-x	文件属组的权限（用户组可以包括多个用户）
r--	其他用户的权限

创建新文件有默认权限，根据umask值计算，属主和属组根据当前进程的用户来设定
```



目录文件权限的表达方式

```bash
x	进入目录
rx	显示目录内的文件名
wx	修改目录内的文件名
```



文件权限的修改方式

```bash
# chmod 修改文件、目录权限（ch就是change的缩写）
chmod u+x /tmp/testfile # 字符方式
chmod 755 /tmp/testfile # 数字方式

# chown 更改属主、属组

# chgrp 可以单独更改属组。不常用（grp是group的缩写）

```





目录和文件的权限意义并不相同，这是因为目录和文件所记录的数据内容不相同导致的。







Linux 文件权限的重要性：

- 系统保护的功能

关于系统服务的文件只能 root 用户才能读写或者执行，例如 /etc/shadow 这一个账号管理的文件，该文件记录了系统当中所有账号的数据。

- 团队开发软件和数据共享的功能

团队协作中，希望每一个成员都可以使用某一些目录下的文件，对其他人不开放。可以将文件权限设置为 [-rwxrws---] ，只提供给 testgroup 的工作团队使用。（这里最后为 s，之后学习）

- 不设置权限可能带来的危害

开机关机、ADSL 的拨接程序、新增或者删除用户的指令，若是任何人都可以执行的话，系统可能常常出问题。





### 改变文件属性与权限

下面这几个是常用语属主，属组以及其他用户的权限修改的指令

- chown：改变文件属主
- chgrp：改变文件属组
- chmod：改变文件的权限，SUID，SGID，SBIT 等特性

#### 改变文件属主，chown

```bash
# chown，用户必须是存在系统中的账号，也就是在/etc/passwd这个文件中有记录的用户名才能改变
# 将hello.txt文件的属主改为bin用户
chown bin hello.txt

# 将hello.txt文件的属主和属组改回root
chown root:root hello.txt
```



复制文件给其他用户时，复制行为（cp）会复制执行者的属性和权限。所以还必须修改文件的属主和属组。

```bash
# cp 来源文件 目标文件
cp .bashrc .bashrc_test

ls -al .bashrc*

```





#### 改变文件属组，chgrp

需要改变的组名必须在 /etc/group 中存在才行。







#### 改变权限，chmod

- 数字类型改变文件权限

```bash

```







- 字符类型改变文件权限



| chmod | u    | +    | r    | 文件或者目录 |
| ----- | ---- | ---- | ---- | ------------ |
|       |      |      |      |              |









```
例题：假设有个账号名称为 dmtsai，他的家目录在/home/dmtsai/，dmtsai 对此目录具有[rwx]的权限。 若在此目录下有个 名为 the_root.data 的文件，该文件的权限如下：

-rwx------ 1 root root 4365 Sep 19 23:20 the_root.data

请问 dmtsai 对此文件的权限为何？可否删除此文件？
答：如上所示，由于 dmtsai 对此文件来说是『others』的身份，因此这个文件他无法读、无法编辑也无法执行， 也就 是说，他无法变动这个文件的内容就是了。
但是由于这个文件在他的家目录下， 他在此目录下具有 rwx 的完整权限，因此对于 the_root.data 这个『档名』来 说，他是能够『删除』的！ 结论就是，dmtsai 这个用户能够删除 the_root.data 这个文件！

假设有个莫名其妙的人，拿着一个完全密封的文件夹放到你的办公室抽屉中，因为完全密封你也打不开、看不到这个文件夹的内部数据(对文件来说，你没有权限)。 但是因为这个文件夹是放在你的抽屉中，你当然可以拿出/放入任何数据在这个抽屉中(对目录来说，你具有所有权限)。 所以，情况就是：你打开抽屉、拿出这个没办法看到的文件夹、将他丢到走廊上的垃圾桶！搞定了 (顺利删除！)！
```













## 3. Linux文件与目录管理





















## 4. Linux磁盘与文件系统管理













## 5. 文件与文件系统的压缩，打包与备份

文件压缩的原理：

1 byte = 8 bits ，每个 byte 当中会有 8 个空格，而每个空格可以是 0, 1。

简单的说，你可以将他想成，其实文件里面有相当多的『空间』存在，并不是完全填满的， 而『压缩』的技术就是将这些『空间』填满，以让整个文件占用的容量下降！ 不过，这些『压缩过的文件』并无法直接被我们的操作系统所使用的，因此， 若要使用这些被压缩过的文件数据，则必须将他『还原』回来未压缩前的模样， 那就是所谓的『解压缩』啰！而至于压缩后与压缩的文件所占用的磁盘空间大小， 就可以被称为是『压缩比』啰！



目前很多的 WWW 网站也是利用文件压缩的技术来进行数据的传送，好让网站带宽的可利用率上升喔！网站上面『看的到的数据』在经过网络传输时，使用的是『压缩过的数据』， 等到这些压缩过的数据到达你的计算机主机时，再进行解压缩，由于目前的计算机指令周期相当的快速， 因此其实在网页浏览的时候，时间都是花在『数据的传输』上面，而不是 CPU 的运算啦！如此一来，由于压缩过的数据量降低了，自然传送的速度就会增快不少！



### 5.1 Linux系统常见的压缩指令



```bash
*.Z			compress程序压缩的文件
*.zip		zip程序压缩的文件
*.gz		gzip程序压缩的文件
*.bz2		bzip2程序压缩的文件
*.xz		xz程序压缩的文件
*.tar		tar程序打包的数据，并没有压缩过
*.tar.gz	tar程序打包的文件，gzip程序压缩的文件
*.tar.bz2	tar程序打包的文件，bzip2程序压缩的文件
*.tar.xz	tar程序打包的文件，xz程序压缩的文件
```

















## 6. 认识vim编辑器

















## 7. 认识和学习BASH













## 8. 正则表达式与文件格式化处理











## 9. 学习Shell脚本











## 系统管理

网络管理、进程管理





### 网络管理

- 网络状态查看

net-tools VS iproute(CentOS7 之后推荐的版本)

```bash
# net-tools，CentOS7之前
ifconfig
route
netstat


# iproute2，CentOS7之后
ip
ss
```





```bash
ifconfig

eth0第一块网卡（网络接口）
你的第一个网络接口可能叫做下面的名字
eno1	板载网卡
ens33	PCI-E网卡
enp0s3	无法获取物理信息的 PCI-E 网卡
CentOS7 使用了一致性网络设备命名，以上都不匹配则使用eth0
```

网络接口命名修改（装换为eth0）

网络命名规则受 `biosdevname` 和 `net.ifnames` 两个参数的影响，编辑 `/etc/default/grub` 文件，增加 `biosdevname=0` 和 `net.ifnames=0` 两个设置，使用 `grub2-mkconfig -o /boot/grub2/grub.cfg` 更新 grub ，重启 `reboot` 即可。



```
GRUB_TIMEOUT=5
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="crashkernel=auto rd.lvm.lv=VolGroup/lv_root rd.lvm.lv=VolGrou    p/lv_swap rhgb quiet"
GRUB_DISABLE_RECOVERY="true"
```



在第六行加入

```
GRUB_CMDLINE_LINUX="crashkernel=auto rd.lvm.lv=VolGroup/lv_root rd.lvm.lv=VolGrou    p/lv_swap rhgb quiet biosdevname=0 net.ifnames=0"
```





|       | biosdevnames | net.ifnames | 网卡名 |
| ----- | ------------ | ----------- | ------ |
| 默认  | 0            | 1           | ens33  |
| 组合1 | 1            | 0           | em1    |
| 组合2 | 0            | 0           | eth0   |

查看网卡物理连接情况

mii-tool eth0



查看网关

route -n

使用 -n 参数不解析主机名



- 网络配置







- 路由命令





- 网络故障排除





- 网络服务管理





- 常用网络配置文件































