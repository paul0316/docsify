# MySQL

> 需要掌握的知识点：
>
> MySQL服务器安装与配置
>
> MySQL客户端使用
>
> 用户权限管理
>
> SQL语句的类型
>
> Select表单查询
>
> 排序，聚合查询
>
> 创建和管理表
>
> 约束管理
>
> DML操作
>
> 内连接操作
>
> 外连接操作
>
> 自连接操作
>
> 子查询
>
> 常用函数
>
> 分页查询
>
> MySQL架构
>
> 存储引擎
>
> SQL优化总体思路
>
> 通用查询日志
>
> 错误日志
>
> 二进制日志
>
> 慢查询日志
>
> 执行计划
>
> 索引及优化策略
>
> 数据库设计



SQL VS NoSQL

```
# SQL
MySQL
Oracle
SQLServer
PostGreSQL

# NoSQL
HBase
MongoDB
Redis
Hadoop
```

特点不一样

关系型数据库

- 数据结构化存储在二维表中

| 姓名 | 性别 | 生日       | 注册时间   |
| ---- | ---- | ---------- | ---------- |
| 张三 | 男   | 1990-01-01 | 2008-01-01 |
| 李四 | 男   | 1990-01-01 |            |
| 王五 | 男   | 1990-01-01 |            |



- 







## 概述











## 安装









## 1. MySQL的架构介绍

### 1.1 MySQL简介

MySQL 是一种关系型数据库，由瑞典 MySQL AB 公司开发，后来被 Oracle 公司收购。在Java企业级开发中非常常用，因为 MySQL 是开源免费的，并且方便扩展。阿里巴巴数据库系统也大量用到了 MySQL，因此它的稳定性是有保障的。MySQL是开放源代码的，因此任何人都可以在 GPL(General Public License) 的许可下下载并根据个性化的需要对其进行修改。MySQL的默认端口号是**3306**。



#### 1.1.1 MySQL内核







#### 1.1.2 SQL优化攻城狮





#### 1.1.3 MySQL服务器的优化





#### 1.1.4 各种参数常量设定





#### 1.1.5 查询语句优化







#### 1.1.6 主从复制







#### 1.1.7 软硬件升级







#### 1.1.8 容灾备份





#### 1.1.9 SQL编程





### 1.2 MySQL Linux版本的安装

使用 rpm 的安装 MySQL 5.5

```bash
# 下载地址
MySQL-client-5.5.48-1.linux2.6.i386.rpm
MySQL-server-5.5.48-1.linux2.6.i386.rpm

# 查看当前系统是否以及安装MySQL
rpm -qa | grep -i mysql

# opt目录下
# 安装MySQL客户端
rpm -ivh MySQL-server-5.5.48-1.linux2.6.i386.rpm
rpm -ivh MySQL-client-5.5.48-1.linux2.6.i386.rpm

# 查看MySQL是否安装成功
rpm -qa | grep mysql
cat /etc/passwd | grep mysql
cat /etc/group | grep mysql
mysqladmin --version

ps -ef | grep mysql

# MySQL服务的启动和停止
service mysql start
service mysql stop

# MySQL默认没有密码，要安装server中的提示修改登录密码
/usr/bin/mysqladmin -u root password 123456

# 自启动MySQL服务
systemctl enable mysql
chkconfig mysql on # 设置MySQL开机启动
chkconfig -list | grep mysql

ntsysv

# 修改配置文件

```















### 1.3 MySQL配置文件







### 1.4 MySQL逻辑架构介绍









### 1.5 MySQL存储引擎







## 2. 索引优化分析



### 2.1 性能下降SQL慢执行时间长等待时间长





### 2.2 常见通用的 Join 查询





### 2.3 索引简介



### 2.4 性能分析



### 2.5 索引优化











## 3. 查询截取分析



### 3.1 查询优化







### 3.2 慢查询日志







### 3.3 批量数据脚本









### 3.4 Show Profile







### 3.5 全局查询日志













## 4. MySQL锁机制























## 5. 主从复制





### 5 .1 















































































