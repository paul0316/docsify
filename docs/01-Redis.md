# Redis

## 概念

redis是一款高性能的 NOSQL 系列的非关系型数据库

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

















































































































































































































































