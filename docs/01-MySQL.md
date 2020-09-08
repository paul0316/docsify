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









## 1. MySQL的架构介绍

### 1.1 MySQL简介

MySQL 是一种关系型数据库，由瑞典 MySQL AB 公司开发，后来被 Oracle 公司收购。在Java企业级开发中非常常用，因为 MySQL 是开源免费的，并且方便扩展。阿里巴巴数据库系统也大量用到了 MySQL，因此它的稳定性是有保障的。MySQL是开放源代码的，因此任何人都可以在 GPL(General Public License) 的许可下下载并根据个性化的需要对其进行修改。MySQL的默认端口号是**3306**。

### 1.2 MySQL Linux版本的安装

有四种方式，源码安装，yum安装，二进制文件编译安装，rpm安装

- 二进制源码安装



- 使用 rpm 的方式安装 MySQL 5.5（Redhat ）

```bash
# 下载地址
MySQL-client-5.5.48-1.linux2.6.i386.rpm
MySQL-server-5.5.48-1.linux2.6.i386.rpm

# 查看当前系统是否以及安装MySQL
rpm -qa | grep -i mysql

# opt目录下
# 安装MySQL客户端，-ivh是加上日志进度条，i是install，v是日志，h是进度条
#-i 安装软件
#-t 测试安装，不是真的安装
#-p 显示安装进度
#-f 忽略任何错误
#-U 升级安装
#-v 检测套件是否正确安装
rpm -ivh MySQL-server-5.5.48-1.linux2.6.i386.rpm --nodeps --force
rpm -ivh MySQL-client-5.5.48-1.linux2.6.i386.rpm --nodeps --force

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

# 设置卡机自启MySQL服务
systemctl enable mysql
chkconfig mysql on # 设置MySQL开机启动
chkconfig --list | grep mysql

# 
cat /ect/inittab

# 可以选择自启动服务的选项
ntsysv

# 修改配置文件的位置

```



参考资料：

https://juejin.im/post/6844903870053761037#heading-31

https://www.cnblogs.com/youjiao/p/12907404.html



可以看到，mysql用户的Shell属性是/bin/false，这意味着：false - do nothing, unsuccessfully。可用usermod命令来修改mysql用户的Shell属性，解锁Shell。 



| 路径              | 解释                    | 备注                       |
| ----------------- | ----------------------- | -------------------------- |
| /usr/lib/mysql    | MySQL数据库文件存放位置 |                            |
| /usr/share/mysql  | 配置文件目录            | mysql.server命令及配置文件 |
| /usr/bin          | 相关命令目录            | mysqladmin mysqldump等命令 |
| /usr/init.d/mysql | 启动停止相关脚本        |                            |





修改字符集和数据存储的位置

修改 `/etc/my.cnf` 配置文件，添加如下内容。

```bash
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
socket=/var/lib/mysql/mysql.sock

[mysqld]
skip-name-resolve
# 设置3306端口
port=3306
socket=/var/lib/mysql/mysql.sock
# 设置mysql的安装⽬录
basedir=/usr/local/mysql
# 设置mysql数据库的数据的存放目录
datadir=/usr/local/mysql/data
# 允许最大连接数
max_connections=200
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使⽤的默认存储引擎
default-storage-engine=INNODB
lower_case_table_names=1
max_allowed_packet=16M
```



修改字符集





### 1.3 MySQL配置文件

主要配置文件

- 二进制日志 log-bin

主要用于主从复制，把主机上的变化记录下来。

```bash
# Replication Master Server (default)
# binary logging is required for replication
log-bin=mysql-bin
```





- 错误日志 log-error

**默认关闭**，记录严重的警告和错误信息，每次启动和关闭的详细信息等。





- 查询日志 log

**默认关闭。**记录查询的 sql 语句，如果开启会降低 MySQL 的整体性能，因为记录日志



- 数据文件

```bash
# Windows：

# Linux
/var/lib/mysql
```



frm 文件：存放表结构



myd 文件：存放表数据data



myi 文件：存放表索引index







- 如何配置

Windows——my.ini 文件

Linux——/etc/my.cnf 文件







### 1.4 MySQL逻辑架构介绍

总体概览

1. 连接层（JDBC，ODBC等等）

MySQL的最上层是连接服务，引入了线程池的概念，允许多台客户端连接。主要工作是：**连接处理、授权认证、安全防护等**。

连接层为通过安全认证的接入用户提供线程，同样，在该层上可以实现基于SSL 的安全连接。

2. 服务层（）

服务层用于处理核心服务，如标准的SQL接口、查询解析、SQL优化和统计、全局的和引擎依赖的缓存与缓冲器等等。所有的与存储引擎无关的工作，如过程、函数等，都会在这一层来处理。在该层上，服务器会解析查询并创建相应的内部解析树，并对其完成优化，如确定查询表的顺序，是否利用索引等，最后生成相关的执行操作。如果是SELECT 语句，服务器还会查询内部的缓存。如果缓存空间足够大，这样在解决大量读操作的环境中能够很好的提升系统的性能。

3. 引擎层（MyISAM、InnoDB等）

存储引擎层，存储引擎负责实际的MySQL数据的**存储与提取**，**服务器通过API 与 存储引擎进行通信**。不同的存储引擎功能和特性有所不同，这样可以根据实际需要有针对性的使用不同的存储引擎。

4. 存储层

数据存储层，主要是将数据存储在运行与裸设备的文件系统之上，并完成与存储引擎的交互。



![image-20200902004928505](C:\Users\Paul\Pictures\typora\image-20200902004928505.png)



总结：和其他数据相比，MySQL 有些与众不同，他的架构可以在不同的场景中应用并发挥良好作用。主要体现在存储引擎的架构上，插件式的存储引擎架构将查询处理和其他系统任务以及数据的存储提取相分离。这种架构可以根据业务的需求和实际需要选择合适的存储引擎。











查询说明







### 1.5 MySQL存储引擎





查看命令

```mysql
show engines;

+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| Engine             | Support | Comment                                                        | Transactions | XA   | Savepoints |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| InnoDB             | DEFAULT | Supports transactions, row-level locking, and foreign keys     | YES          | YES  | YES        |
| CSV                | YES     | CSV storage engine                                             | NO           | NO   | NO         |
| MyISAM             | YES     | MyISAM storage engine                                          | NO           | NO   | NO         |
| BLACKHOLE          | YES     | /dev/null storage engine (anything you write to it disappears) | NO           | NO   | NO         |
| PERFORMANCE_SCHEMA | YES     | Performance Schema                                             | NO           | NO   | NO         |
| MRG_MYISAM         | YES     | Collection of identical MyISAM tables                          | NO           | NO   | NO         |
| ARCHIVE            | YES     | Archive storage engine                                         | NO           | NO   | NO         |
| MEMORY             | YES     | Hash based, stored in memory, useful for temporary tables      | NO           | NO   | NO         |
| FEDERATED          | NO      | Federated MySQL storage engine                                 | NULL         | NULL | NULL       |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+

```





MyISAM 和 InnoDB 对比

| 对比项   | MyISAM                                                   | InnoDB                                                       |
| -------- | -------------------------------------------------------- | ------------------------------------------------------------ |
| 主外键   | 不支持                                                   | 支持                                                         |
| 事务     | 不支持                                                   | 支持                                                         |
| 行表锁   | 表锁，即使操作一条记录也会锁住整个表，不适合高并发的操作 | 行锁，操作时只锁住某一行，不对比其他行有影响。适合高并发的操作。 |
| 缓存     | 只缓存索引，不缓存真实数据                               | 不仅缓存索引还要缓存真实数据。对性能内存要求较高，而且缓存的内存大小对性能有决定性的影响 |
| 表空间   | 小                                                       | 大                                                           |
| 关注点   | 性能                                                     | 事务                                                         |
| 默认安装 | Y                                                        | Y                                                            |





阿里巴巴、淘宝用的哪一个？

![image-20200902010319934](C:\Users\Paul\Pictures\typora\MySQL\001)











## 2. 索引优化分析



### 2.1 性能下降SQL慢执行时间长等待时间长



**查询语句写的不好**





**索引失效**

- 单值索引

```mysql
id name email weixinNumber

select * from user where name = 'paul';

# 在user表的name字段建立索引，建立索引后会给我们排序，这样速度就会提升
# 举例：就像拍照的时候按照身高来排序
create index idx_user_name on user(name)
```



- 复合索引

```mysql
id name email weixinNumber

select * from user where name = '' and email = '';

# 在user表的name和email字段建立索引
# 举例：就像淘宝京东可以选取多个限定条件
create index idx_user_nameEmail on user(name, email);
```

到底哪些字段需要建立索引？







**关联查询太多 join（设计缺陷或者不得以的需求）**



**服务器调优及各个参数的设置（缓冲、线程数等）**





> 类似的问题：你做过 JVM 的调优吗？



### 2.2 常见通用的 Join 查询



#### SQL 执行顺序

手写的顺序

```mysql
SELECT DISTINCT
	< select_list >
FROM
	< left_table > < join_type >
JOIN < right_table > ON < join_condition >
WHERE
	< where_condition >
GROUP BY
	< group_by_list >
HAVING
	< having_condition >
ORDERED BY
	< order_by_condition >
LIMIT < limit_number >
```



机读的顺序

```mysql
FROM < left_table > < join_type >
ON < join_condition > JOIN < right_table >
WHERE < where_condition >
GROUP BY < group_by_list >
HAVING < having_condition >
SELECT
DISTINCT < select_list >
ORDERED BY < order_by_condition >
LIMIT < limit_number >
```

SQL 解析的顺序

![image-20200902091731422](C:\Users\Paul\Pictures\typora\MySQL\002.png)

先从from读起



总结







#### Join图







#### 建表 SQL









#### 7种JOIN



![image-20200902133310478](C:\Users\Paul\Pictures\typora\MySQL\003.png)



```mysql
select <selec_list> from table A A inner join table B B on A.Key = B.Key
```



A 独有的数据

```mysql
select <select_list> from table A A left join table B B on A.Key = B.Key where B.Key is null;
```



B 独有的数据

```mysql
select <select_list> from table A A right join table B B on A.key = B.Key where A.Key is null;
```



inner join

left join

right join

full outer join 



```mysql
select <select_list> from table A A full outer join table B B on A.Key = B.Key where A.Key is null or B.Key is null;
```



使用 union 来去掉重复的部分

```mysql
select <select_list> from table A A left join table B B on A.Key = B.Key
union
select <select_list> from table A A right join table B B on A.Key = B.Key;
```

下述语句等同于

```mysql
select <select_list> from table A A full outer join table B B on A.Key = B.Key;
```





### 2.3 索引简介

> Indexes are used to find rows with specific column values quickly. Without an index, MySQL must begin with the first row and then read through the entire table to find the relevant rows. The larger the table, the more this costs. If the table has an index for the columns in question, MySQL can quickly determine the position to seek to in the middle of the data file without having to look at all the data. This is much faster than reading every row sequentially.

MySQL 索引是帮助 MySQL 高效获取数据的**数据结构**。

为什么需要索引？

事先对要查询的数据进行排序操作，查询时的效率就会大大提升。

索引会影响查询语句后的 `where` 、 `order by` 。 



索引是如何工作的？（详解）

Binary-Tree索引，使用二叉查找树

数据本身之外，数据库还维护着一个满足特定查找算法的数据结构，这些数据结构以某种方式指向数据，这样就恶意在这些数据结构的基础上实现高级查找算法，这种数据结构就是索引。

![image-20200902144616741](C:\Users\Paul\Pictures\typora\MySQL\004.png)

为了加快col2的查找，可以维护一个上图所示的二叉查找树，每个节点分别包括索引键值和一个指向对应数据记录的物理地址的指针，这样就可以运用二叉查找树更快的查到到需要的数据。



实际开发种，逻辑删除了数据，但是物理上只是把数据的激活状态更改为非激活状态。Java 的 service 调用的是 delete，但是实际数据库操作的是 update。第一是便于记录所有的操作，进行数据分析；第二是便于索引。

一般来说，索引本身也很大，不可能全部存储在内存中，因此索引往往是以文件的方式存储在磁盘上。我们平常说的索引，如果没有特别指明，都是B树结构的索引。其中聚集索引，次要索引，覆盖索引，复合索引，前缀索引，唯一索引默认都是使用B+树索引。除了B+树索引，还有哈希索引（hash index）等。



优势

- 提高了数据检索的效率，降低数据库 IO 成本。

- 对数据进行排序，降低数据排序的成本，降低了 CPU 的消耗。



劣势

- 实际上索引也是一张表，保存了逐渐和索引字段，并指向实体表的记录，索引也是要占用空间的
- 虽然索引大大提升了查询速度，同时会降低更新表的速度，如对表进行insert，update和delete。因为更新表时，MySQL不仅要保存数据，还要保存一下索引文件每次更新添加了索引列的字段，都会调整因为更新带来的键值变化后的索引信息
- 





索引的分类：单值索引、唯一索引、复合索引等。

单值索引：几个索引只包含单个列，一个可以有多个单列索引。

唯一索引：索引列的值必须唯一，但允许有空值。

复合索引：一个索引包含几个列。



```
# 基本语法

# 创建
create [unique] index indexName on mytable(columnName(length));
alter mytable add [unique] index [indexName] on (columnName(length));

# 删除
drop index [indexName] on mytable;

# 查看
show index from table_name\G

# 使用ALTER命令：有四种方式添加数据库的索引
alter table tbl_name add primary key (column_list)  # 该语句添加一个主键，索引必须时唯一的，且不能为null

alter table tbl_name add unique index_name (column_list)  # 创建索引的值必须时唯一的（除了null外，null可以出现多次）

alter table tbl_name add index index_name (column_list)  # 添加普通索引，索引值可以出现多次

alter table tbl_name add fulltext index_name (column_list)  # 指定索引为fulltext，用于全文索引
```



MySQL 索引结构



- B-Tree 索引
- Hash 索引
- full-text 全文索引
- R-Tree 索引





![image-20200902152344574](C:\Users\Paul\Pictures\typora\MySQL\005.png)





哪些情况需要创建索引？

1. 主键自动建立唯一索引
2. 频繁作为查询条件的字段应该创建索引
3. 查询中与其他表关联的字段，外键关系建立索引
4. 频繁更新的字段不适合创建索引——因为每次更新不单单更新了记录还会更新索引
5. where 条件里用不到的字段不创建索引
6. 单键/组合索引的选择问题——在高并发下倾向创建组合索引
7. 查询中排序的字段，排序字段若通过索引去访问将大大提高排序速度
8. 查询中统计或者分组字段



哪些情况不要创建索引？

1. 表记录太少（500万以上）
2. 经常增删改的字段
3. 如果某个数据列包含许多重复的内容，为它建立索引没有太大的实际效果







### 2.4 性能分析（重要）

#### MySQL Query Optimizer











#### MySQL 常见瓶颈

主要瓶颈有三种：


CPU 在饱和的时候一般发生在数据装入内存或者从磁盘读取数据的时候



磁盘 I/O 瓶颈发生在装入数据远大于内存容量的时候



服务器硬件的性能瓶颈：top，free，iostat 和 vmstat 来查看系统的性能状态







#### Explain



**是什么？（查看执行计划）**

使用 explain 关键字可以模拟优化器执行 SQL 查询语句，从而知道

在日常工作中，我们会有时会开慢查询去记录一些执行时间比较久的 SQL 语句，找出这些SQL语句并不意味着完事了，些时我们常常用到 explain 这个命令来查看一个这些 SQL 语句的执行计划，查看该 SQL 语句有没有使用上了索引，有没有做全表扫描，这都可以通过 explain 命令来查看。所以我们深入了解 MySQL 的基于开销的优化器，还可以获得很多可能被优化器考虑到的访问策略的细节，以及当运行 SQL 语句时哪种策略预计会被优化器采用。（QEP：sql生成一个执行计划 query Execution plan）





Explain + SQL 语句

```mysql
explain select * from table;
```









**能干嘛？**



- 表的读取顺序

id 是 select 查询的序列号，包含一组数字，表示查询中执行 select 子句或操作表的顺序。

id 相同的情况，执行顺序由上至下的

id 不同的情况，id 的序号会增加，id越大优先级要高，优先执行。

id 相同不同，同时存在



- 数据读取操作的操作类型





- 哪些索引可以使用





- 哪些索引被实际使用





- 表之间的引用





- 每张表有多少行被优化器查询







**各字段解释**



id相同的情况，执行顺序由上至下的

id不同的情况，id的序号会增加，id越大优先级要高，优先执行。

同时存在，



select_type

常见的值有六种：

| id   | select_type  | 描述                                                         |
| ---- | ------------ | ------------------------------------------------------------ |
| 1    | SIMPLE       | 简单的 select 查询，查询不包含子查询或者 UNION               |
| 2    | PRIMARY      | 查询中包含任何复杂的子部分，最外层查询则被标记为 PRIMARY     |
| 3    | SUBQUERY     | 在 select 或者 where 列表中包含了子查询                      |
| 4    | DERIVED      | 在 from 列表中包含的子查询被标记为 DERIVED(衍生)。MySQL会递归执行这些子查询，把结果放在临时表中。 |
| 5    | UNION        | 若第二个 select 出现在 union 之后，则被标记为 union；若 union 包含在 from 自居的子查询中，外层 select 被标记为：DERIVED。 |
| 6    | UNIOB RESULT | 从 union 表获取结果的 select                                 |





table







**type 显示的是查询使用的何种类型**，是较为重要的一个指标，结果值从最好到最坏依次是：

system>const>eq_ref>ref>fulltext>ref_or_null>index_merge>unique_subquery>index_subquery>range>index>ALL

system>const>eq_ref>ref>range>index>ALL

一般来说，要保证查询至少达到 range 的级别，最好能达到 ref。

| 访问类型 | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| system   | 只有一行记录（等于系统表），这是 cons 类型的特列，平时不会出现，这个也可以忽略不计。 |
| const    | **表示通过索引一次就找到了**，const 用于比较 primary key 或者 unique 索引。因此只匹配一行数数据，所以很快。如将主键放置于 where 列表中，MySQL 就能将该查询转换为一个常量。`select * from (select * from t1 where id = 1) d1` |
| eq_ref   | **唯一性索引扫描**，对于每个索引键，表中只有一条记录与之匹配。常见于主键或者唯一索引。 |
| ref      | **非唯一性索引扫描**，返回匹配某个单独值的所有行。本质上也是一种索引访问，它返回所有匹配某个单独值的行，然而，他可能会找到多个符合条件的行，所以它应该属于查找和扫描的混合体。 |
| range    | **只检索给定范围的行，使用一个索引来选择行。**key 列显示使用了哪个索引，一般就是在你的 where 语句中出现了 between、<、>、in 等查询。这种范围扫描索引比全表扫描要好，因为它只需要开始与索引的某一个点，而结束与另一点，不用扫描全部索引。 |
| index    | **Full Index Scan，index 与 ALL 区别为 index 类型只遍历索引树。**这通常比 ALL 块，因为索引文件通常比数据文件小。（也就是说 ALL 和 index 都是读全表，但是 index 是从索引中读取的，而 ALL 是从硬盘中读取的） |
| all      | Full Table Scan，将遍历全表来找到匹配的行。                  |



possible_keys 显示可能应用在这张表中的索引，一个或者多个。查询涉及到的字段上若存在索引，则该索引将被列出，但不一定被查询实际使用。





key 是实际使用到的索引。如果为 null，则没有使用索引。查询中若使用了覆盖索引，则该索引仅出现在 key 列表中。





key_len 表示索引中使用的字节数，可通过该列计算查询中使用的索引的长度。在不损失精确性的情况下，长度越短越好。key_len 显示的值为索引的最大可能长度，并非实际使用长度，即 key_Len 是根据表定义而计算的，不是通过表内检索出的





ref **表示上述表的连接匹配条件，即哪些列或常量被用于查找索引列上的值**





rows **表示MySQL根据表统计信息及索引选用情况，估算的找到所需的记录所需要读取的行数**







Extra **该列包含MySQL解决查询的详细信息,有以下几种情况：**

Using where:列数据是从仅仅使用了索引中的信息而没有读取实际的行动的表返回的，这发生在对表的全部的请求列都是同一个索引的部分的时候，表示mysql服务器将在存储引擎检索行后再进行过滤

Using temporary：表示MySQL需要使用临时表来存储结果集，常见于排序和分组查询

Using filesort：MySQL中无法利用索引完成的排序操作称为“文件排序”

Using join buffer：改值强调了在获取连接条件时没有使用索引，并且需要连接缓冲区来存储中间结果。如果出现了这个值，那应该注意，根据查询的具体情况可能需要添加索引来改进能。

Impossible where：这个值强调了 where 语句会导致没有符合条件的行。

Select tables optimized away：这个值意味着仅通过使用索引，优化器可能仅从聚合函数结果中返回一行







> 不要过度优化，为了优化而优化，数据量少的时候减少不必要的优化。





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















































































