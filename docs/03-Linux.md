# Linux

> 需要掌握：
>
> Linux背景
>
> 系统操作
>
> 服务管理
>
> Shell脚本
>
> 文本操作
>
> 常见服务搭建——DNS，HTTP等，家庭NAS搭建共享存储

参考：极客时间-Linux实战技能100讲



## 1. 背景介绍













## 2. 

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



学习到第13课































































































































































