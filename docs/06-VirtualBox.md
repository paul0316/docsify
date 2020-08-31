# VirtualBox 安装 CentOS 7

Windows 下推荐使用 VirtualBox 来安装虚拟机。



## 安装 VirtualBox

直接进入[官方下载页面](https://www.virtualbox.org/wiki/Downloads)，选择相应的安装包即可，这里选择 Windows hosts 下载。



## 安装 CentOS 7

### 方式1：直接安装











当你安装 `yum -y install net-tools` 的时候，可能会遇到的 `could not find a valid baseurl for repo` 问题

```bash
vi /etc/sysconfig/network-scripts/ifcfg-enp0s3

# 把ONBOOT=no修改为yes
ONBOOT=yes
```



也可以切换源来解决（[关于centos7下yum安装报错问题解决方法Cannot find a valid baseurl for repo: base/7/x86_64](https://blog.csdn.net/qq_41684957/article/details/83345154)）







出现问题：

宿主机可以 ping 通虚拟机，虚拟机可以 ping 通过外网但是不能 ping 通宿主机。

> 特别注意：Windows 防火墙的设置以及宿主机的 IP 设置。

参考资料：

[虚拟机桥接方式连接外网](https://blog.csdn.net/zdh_139/article/details/73456654)

[虚拟机ping不通主机，但是主机可以ping通虚拟机](https://blog.csdn.net/hskw444273663/article/details/81301470)

[建议人手一套：个人专属多节点Linux环境打造，Linux操作系统学习实验环境安装配置视频教程](https://www.bilibili.com/video/BV1bA411b7vs)







通过克隆的方式创建新的虚拟机

参考资料：

[VirtualBox 如何克隆虚拟机并设置网络](https://my.oschina.net/antsky/blog/831574)

克隆结束后需要对网络进行修改。





## 方式2：使用 vagrant 安装

什么是 vagrant？

