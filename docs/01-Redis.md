# Redis

非关系型数据库NoSQL（Not Only SQL，不仅仅是SQL）





为什么使用 NoSQL？



大型网站的架构演变

单机 MySQL

早前网站的访问量一般都不大，用单个数据库完全可以应付。大都是静态网页，动态交互的网站并不是很多。



Memchached（缓存）+ MySQL + 垂直拆分

<img src="https://tva1.sinaimg.cn/large/007S8ZIlly1ghchj22h30j30vq0faae2.jpg" alt="image-20200802151132799" style="zoom:50%;" />



MySQL 主从读写分离

<img src="https://tva1.sinaimg.cn/large/007S8ZIlly1ghchk4emvij30yg0a8402.jpg" alt="image-20200802151234326" style="zoom:50%;" />

Master-Slave 模式

写的操作在主库，读的操作是从库。





分表分库 + 水平拆分 + MySQL 集群

主库的写压力开始出现瓶颈，大量数据的持续猛增，由于 MyISAM 引擎使用表锁，在高并发下会出现严重的锁问题，大量的高并发 MySQL 应用开始使用 InnoDB 引擎代替 MyISAM 引擎。

表锁：

行锁并发性更高

比较活跃的数据放在一起，用得少的数据放在一起

<img src="https://tva1.sinaimg.cn/large/007S8ZIlly1ghcio4ili6j30x00iwjwk.jpg" alt="image-20200802155057978" style="zoom:50%;" />



MySQL 的扩展性瓶颈

MySQL 数据库也经常存储一些大文本字段，导致数据表非常大，做数据库恢复的时候就会很慢，不容易恢复数据库。MySQL 的扩展性差，大数据下 IO 压力大，表结构更改困难，正式当前使用 MySQL 的开发人员面临的问题。



用户---企业级防火墙---Ngnix---App服务器（tomcat）---MySQL/Oracle



为什么使用 NoSQL？

- 易扩展

NoSQL 数据库种类繁多，但是有一个共同的特点都是去掉关系数据库的关系型特性。数据之间无关系，这样就非常容易扩展，这样就为架构带来了可扩展的能力。

- 大数据量高性能

数据库都具有非常高的读写性能，尤其是在大数据量下。

- 多样灵活的数据类型

NoSQL 无需事先为存储的数据建立字段，随时可以存储自定义的数据格式。而在关系型数据库里，增删改查是一件非常麻烦的事情。

- 传统 RDBMS VS NoSQL

<img src="https://tva1.sinaimg.cn/large/007S8ZIlly1ghcj4dhv9jj30gi0mgtd5.jpg" alt="image-20200802160638131" style="zoom:33%;" />



Redis

Memcache

Mongdb



谈谈对 Redis 的理解？

键值对、缓存、持久化等



3V + 3高

大时代的3V

海量Volume

多样Variety

实时Velocity



互联网需求的3高

高并发

高可扩

高性能



当下的应用是 SQL 和 NoSQL 一起使用







## 初识 Redis

>Redis is an open source (BSD licensed), in-memory data structure store, used as a database, cache and message broker. It supports data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs, geospatial indexes with radius queries and streams. Redis has built-in replication, Lua scripting, LRU eviction, transactions and different levels of on-disk persistence, and provides high availability via Redis Sentinel and automatic partitioning with Redis Cluster

- 开源

- 基于键值的存储服务系统

- 多种数据结构

- 高性能、功能丰富

<img src="https://tva1.sinaimg.cn/large/007S8ZIlly1ghbd51kkzzj31hc0u0e36.jpg" alt="image-20200801155404879" style="zoom:33%;" />



```
Java Map ---> String value  = map.get("key")

Python Dict ---> dictionary['key'] = value


```

REmote DIctionary Server（远程字典服务器）



数据结构

字符串

哈希表

链表

集合

有序集合

<img src="https://tva1.sinaimg.cn/large/007S8ZIlly1ghbk1ckp08j313c0msdnp.jpg" alt="image-20200801195245397" style="zoom:33%;" />





### Redis 的特性回顾



- 速度快

官方给出的速度是：10w OPS

数组存在哪？内存当中

<img src="https://tva1.sinaimg.cn/large/007S8ZIlly1ghbk7ujub8j30p60o2agu.jpg" alt="image-20200801195900414" style="zoom:33%;" />

什么语言写的？C语言（50000 lines）

线程模型？单线程

- 持久化（断电不丢失数据）

Redis 所有数据保存在内存当中，对数据的更新将异步地保存到磁盘上。

- 多种数据结构

5种：字符串，哈希表，链表，集合和有序集合。

新的版本还提供了

BitMaps：位图

HyperLogLog：超小内存唯一值计数

GEO：地理位置定位



- 支持多种编程语言





- 功能丰富

发布订阅，Lua 脚本，简单事务功能和 pipline 的功能。



- 简单



- 主从复制

支持数据的备份，即master-slave模式的数据备份



- 高可用、分布式





### Redis 单机安装



建立软连接？

```bash
ln -s redis-3.0.7 redis
```





三种启动方式



```
ps -ef | grep redis

netstat -antpl | grep redis

redis-cli -h ip -p port ping
```



动态参数启动

```
redis-server --port 6380
```



配置文件启动

```
redis-server configPath
```



生成环境选择配置启动

单机多实例配置文件可以用端口区分



redis 客户端连接

```
redis-cli -h 10.10.79.150 -p 6384
```



redis 客户端返回值

状态回复

```
ping

PONG
```



错误回复

```

```



为什么是6379作为端口号？





```bash
To have launchd start redis now and restart at login:
  brew services start redis
Or, if you don't want/need a background service you can just run:
  redis-server /usr/local/etc/redis.conf
```



```bash
3175:C 02 Aug 2020 09:26:20.204 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
3175:C 02 Aug 2020 09:26:20.204 # Redis version=6.0.5, bits=64, commit=00000000, modified=0, pid=3175, just started
3175:C 02 Aug 2020 09:26:20.204 # Configuration loaded
3175:M 02 Aug 2020 09:26:20.206 * Increased maximum number of open files to 10032 (it was originally set to 256).
                _._
           _.-``__ ''-._
      _.-``    `.  `_.  ''-._           Redis 6.0.5 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 3175
  `-._    `-._  `-./  _.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |           http://redis.io
  `-._    `-._`-.__.-'_.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |
  `-._    `-._`-.__.-'_.-'    _.-'
      `-._    `-.__.-'    _.-'
          `-._        _.-'
              `-.__.-'

3175:M 02 Aug 2020 09:26:20.207 # Server initialized
3175:M 02 Aug 2020 09:26:20.208 * Ready to accept connections
```











### Redis 典型使用场景

- 缓存系统

<img src="https://tva1.sinaimg.cn/large/007S8ZIlly1ghbkj25zptj310e0owgsp.jpg" alt="image-20200801200946761" style="zoom:33%;" />

这里的 cache 可以是 redis。



- 计数器









消息队列系统



排行榜



社交网络



实时系统









## API 的理解和使用

### 通用命令

1. 通用命令

```bash
keys

dbsize

exists key

del key [key ...]

expire key second

type key
```



```bash
set hello world
set php good
set java best

key *

key [pattern]
遍历所有的key
```



```bash
# key [pattern]
# 遍历所有的key

127.0.0.1:6382> mset hello world hehe haha php good phe his
OK
127.0.0.1:6382> keys he*
1) "hehe"
2) "hello"
127.0.0.1:6382> keys he[h-l]*
1) "hehe"
2) "hello"
127.0.0.1:6382> keys ph?
1) "phe"
2) "php"
127.0.0.1:6382>
```

keys 命令一般不在生产环境使用

keys*怎么用呢？

- 热备从节点

- scan



dbsize

```bash
# dbsize
# 计算key的总数

127.0.0.1:6382> mset k1 v1 k2 v2 k3 v3 k4 v4
OK
127.0.0.1:6382> dbsize
(integer) 8
127.0.0.1:6382> sadd myset a b c d e
(integer) 5
127.0.0.1:6382> dbsize
(integer) 9
```





exists

```bash
# exists key
# 检查key是否存在

127.0.0.1:6382> set a b
OK
127.0.0.1:6382> exists a
(integer) 1
127.0.0.1:6382> del a
(integer) 1
127.0.0.1:6382> exists a
(integer) 0
```





```bash
# del
# 删除指定的key-value

127.0.0.1:6382> set a b
OK
127.0.0.1:6382> get a
"b"
127.0.0.1:6382> del a
(integer) 1
127.0.0.1:6382> get a
(nil)
```



```bash
# expire key seconds
# key 在 seconds 秒后过期

# ttl key
# 查看key剩余的过期时间

# persist key
# 去掉key的过期时间

127.0.0.1:6382> set hello world
OK
127.0.0.1:6382> expire hello 20
(integer) 1
127.0.0.1:6382> ttl hello
(integer) 18
127.0.0.1:6382> get hello
"world"
127.0.0.1:6382> ttl hello
(integer) 11
127.0.0.1:6382> ttl hello
(integer) 6
127.0.0.1:6382> ttl hello
(integer) -2
127.0.0.1:6382> get hello
(nil)

127.0.0.1:6382> set hello world
OK
127.0.0.1:6382> expire hello 20
(integer) 1
127.0.0.1:6382> ttl hello
(integer) 14
127.0.0.1:6382> persist hello
(integer) 1
127.0.0.1:6382> ttl hello
(integer) -1
127.0.0.1:6382> get hello
"world"
```



```bash
# type
# 返回key的类型

127.0.0.1:6382> set a b
OK
127.0.0.1:6382> type a
string
127.0.0.1:6382> sadd myset 1 2 3
(integer) 3
127.0.0.1:6382> type myset
set
```



时间复杂度

| 命令   | 时间复杂度 |
| ------ | ---------- |
| keys   | O(n)       |
| dbsize | O(1)       |
| del    | O(1)       |
| exists | O(1)       |
| expire | O(1)       |
| type   | O(1)       |







2. 数据结构和内部编码

<img src="https://tva1.sinaimg.cn/large/007S8ZIlly1ghcakqy4j5j30nk0to7fb.jpg" alt="image-20200802111058669" style="zoom: 33%;" />

redisObject

<img src="https://tva1.sinaimg.cn/large/007S8ZIlly1ghcampa1jmj31gc0tugyb.jpg" alt="image-20200802111251814" style="zoom:33%;" />





3. 单线程架构

Redis 单线程为什么这么快？

- 纯内存（主要）

- 非阻塞IO
- 避免线程切换和竞态消耗







| 结构类型 | 结构存储的值                                                 | 结构的读写能力 |
| -------- | ------------------------------------------------------------ | -------------- |
| String   | 可以是字符串、整数和浮点数                                   |                |
| List     | 一个链表，链表上的每一个节点都包含一个字符串                 |                |
| Set      | 包含字符串的无序收集器，并且包含的每个字符串都是独一无二、各不相同的 |                |
| Hash     | 包含键值对的无序散列表                                       |                |
| ZSet     | 字符串成员与浮点数分值之间的有序映射，元素的排列顺序由分值的大小决定 |                |





### 字符串类型（String）



<img src="https://tva1.sinaimg.cn/large/007S8ZIlly1ghcgk2vtp7j30j80c4dgw.jpg" alt="image-20200802143755605" style="zoom:33%;" />



场景

- 缓存
- 计数器
- 分布式锁



```bash
get key
set key value
del key
```



```bash
incr

decr

incrby

decrby


```







### 哈希类型（Hash）



<img src="https://tva1.sinaimg.cn/large/007S8ZIlly1ghcgjmx57hj30ks0fomzw.jpg" alt="image-20200802143730311" style="zoom:33%;" />



| 命令    | 行为                                       |
| ------- | ------------------------------------------ |
| HSET    | 在散列里面关联起给定的键值对               |
| HGET    | 获取指定散列键的值                         |
| HGETALL | 获取散列包含的所有键值对                   |
| HDEL    | 如果给定的键存在于散列里面，那么移除这个键 |



```bash
127.0.0.1:6382> hset hash-key sub-key1 value1
(integer) 1
127.0.0.1:6382> hset hash-key sub-key2 value2
(integer) 1
127.0.0.1:6382> hset hash-key sub-key value
(integer) 1
127.0.0.1:6382> hset hash-key sub-key1 value1
(integer) 0
127.0.0.1:6382> hgetall hash-key
1) "sub-key1"
2) "value1"
3) "sub-key2"
4) "value2"
5) "sub-key"
6) "value"
127.0.0.1:6382> hdel hash-key sub-key
(integer) 1
127.0.0.1:6382> hdel hash-key sub-key
(integer) 0
127.0.0.1:6382> hgetall hash-key
1) "sub-key1"
2) "value1"
3) "sub-key2"
4) "value2"
127.0.0.1:6382> hget hash-key sub-key1
"value1"
```















### 列表类型（List）



<img src="https://tva1.sinaimg.cn/large/007S8ZIlly1ghcgkjjke0j30jw0d0jth.jpg" alt="image-20200802143821076" style="zoom:33%;" />



| 命令   | 行为                                       |
| ------ | ------------------------------------------ |
| RPUSH  | 将给定值推入列表的右端                     |
| LRANGE | 获取列表在给定值上的所有值                 |
| LINDEX | 获取列表在给定位置上的单个元素             |
| LPOP   | 从列表的左端弹出一个值，并且返回被弹出的值 |

```bash
127.0.0.1:6382> rpush list-key item
(integer) 1
127.0.0.1:6382> rpush list-key item2
(integer) 2
127.0.0.1:6382> rpush list-key item
(integer) 3
127.0.0.1:6382> lrange list-key 0 -1
1) "item"
2) "item2"
3) "item"
127.0.0.1:6382> lindex list-key 1
"item2"
127.0.0.1:6382> lindex list-key -1
"item"
127.0.0.1:6382> lindex list-key 0
"item"
127.0.0.1:6382> lpop list-key
"item"
127.0.0.1:6382> lrange list-key 0 -1
1) "item2"
2) "item"
```





### 集合类型（Set）



<img src="https://tva1.sinaimg.cn/large/007S8ZIlly1ghcgl20kn5j30jc0cw765.jpg" alt="image-20200802143852603" style="zoom:33%;" />



| 命令      | 行为                                         |
| --------- | -------------------------------------------- |
| SADD      | 将给定元素添加到集合                         |
| SMEMBERS  | 返回集合包含的所有元素                       |
| SISMEMBER | 检查给定元素是否存在于集合中                 |
| SREM      | 如果给定的集合存在于集合中，那么移除这个元素 |





### 有序集合类型（ZSet）



<img src="https://tva1.sinaimg.cn/large/007S8ZIlly1ghcgiyhcyvj30ju0hoq6r.jpg" alt="image-20200802143647487" style="zoom:33%;" />



| 命令          | 行为                                                       |
| ------------- | ---------------------------------------------------------- |
| ZADD          | 将一个带有给定成分值得成员添加到有序集合中                 |
| ZRANGE        | 根据元素在有序排列中所处的位置，从有序集合里面获取多个元素 |
| ZRANGEBYSCORE | 获取有序集合在给定范围内的所有元素                         |
| ZREM          | 如果给定成员存在于有序集合，那么移除这个元素               |



```bash
127.0.0.1:6382> zadd zset-key 728 member1
(integer) 1
127.0.0.1:6382> zadd zset-key 982 member0
(integer) 1
127.0.0.1:6382> zrange zset-key 0 -1 withscores
1) "member1"
2) "728"
3) "member0"
4) "982"
```





Redis 使用场景

投票系统，比如Reddit，StackOverflow



使用两个有序集合来存储文章：第一个，有序集合的成员为文章ID，分值为文章的发布时间；第二个有序集合的成员同样是文章ID，分值为文章的评分。这样网站既可以根据文章发布的先后顺序来展示，又可以根据文章的评分来展示。





解析配置文件 redis.conf 文件











## Redis 客户端的使用









## 瑞士军刀 Redis











## Redis 持久化的取舍和选择









## Redis 复制的原理和优化











## Redis Sentinel













## Redis Cluster











## 下载安装

官网：https://redis.io/
中文网：https://www.redis.net.cn/


## 命令操作

### 数据结构

redis存储的是：key，value格式的数据，其中key都是字符串，value有5中不同的数据结构
value的数据结构

- 字符串类型 string
- 哈希类型 hash：map格式
- 列表类型 list：linkedlist格式
- 集合类型 set
- 有序集合类型 sortedset

字符串类型

1. 存储：set key value
2. 获取：get key
3. 删除：del key

哈希类型

1. 存储：hset key field value
2. 获取：
   hget key field value：获取指定的field对应的值
   hgetall key：获取所有的field和value
3. 删除：hdel key field

列表类型

1. 存储：
   lpush key value
   rpush key value
2. 获取：
   lrange key start end：范围获取
3. 删除：
   lpop key：删除列表最左边的元素，并将元素返回
   rpop key：删除列表最右边的元素，并将元素返回

集合类型：不允许重复元素

1. 存储：sadd key value
2. 获取：smembers key：获取set集合中所有的元素
3. 删除：srem key value：删除set集合中的某个元素

有序列表类型：不允许重复元素，并且元素有顺序

1. 存储：zadd key score value：
2. 获取：zrange key start end：
3. 删除：zrem key value：

通用的命令

1. keys *：查询所有的键
2. type key：获取键对应的value类型
3. del key：删除指定的key value

## 持久化操作

1. redis是一个内存数据库，当redis服务器重启或者电脑重启，数据就会丢失，我们可以将redis内存中的数据持久化保存到硬盘的文件中
2. redis持久化机制
   RDB：默认方式，不需要进行配置
   	在一定的时间间隔内，检测key的变化情况，然后持久化数据
   	1. 编辑 redis.windows.conf文件
   	


	AOF：日记记录的方式，可以记录每一条命令的操作。每一条命令操作后，持久化数据。


## 使用 Java 客户端操作 jedis






### jedis 连接池 JedisPool

使用：

1. 创建 JedisPool 连接池对象
2. 调用方法 getResource() 方法获取 Jedis 的连接

```java
public void test6() {
	// 0. 创建一个配置对象
	JedisPoolConfig config = new JedisPoolConfig();
	config.setMaxTotal(50);
	config.setMaxIdle(10);

	// 1. 创建 Jedis 连接池对象
	JedisPool jedisPool = new JedisPool();

	// 2. 获取连接
	Jedis jedis = jedisPool.getResource();

	// 3. 使用
	jedis.set("hehe", "haha");

	// 4. 关闭 归还到连接池当中
	jedis.close();
}
```

















































































































































































































































