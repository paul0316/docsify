# 数据库设计与JDBC

## 概述





ORM编程思想（object relational mapping）

一个数据表对应一个Java类

表中的一条记录对应Java类的一个对象

表中的一个字段对于java类的一个属性



ORM框架

mybatis







针对Oracle数据库表的通用查询操作

```java
// 针对Oracle表的通用操作
public class OrderFroQuery {
    //
    @Test
    public
    
    
    // 
    public class OrderForQuery() {
        
    }
}
```





```java
import java.sql.Date

public class Order {
    private int orderId;
    private String orderName;
    private Date orderDate;
}
```





问题：

针对与表的字段名与类的属性名不相同的情况：

1. 必须声明sql的时候，使用类的属性名来命名字段的别名

```mysql
# SQLyog中的操作
select order_id orderId, order_name orderName, order_date orderDate from `order` where order_id = 1;
```



```java
// java中的操作
String sql = "select order_id orderId, order_name orderName, order_date orderDate from `order` where order_id = ?";
```



通过给数据库的列名起别名（类的属性名）来解决这个问题。







2. 使用 ResultSetMetaData 的时候，需要使用 getColumnLabel 来替换 getColunm（获取的是数据库表的列名，但不是我们起的别名），获取列的别名。



图解查询操作的流程

把数据库表中的一条记录封装为 java 的一个对象



```

```



![image-20200717161102935](C:\Users\Paul\AppData\Roaming\Typora\typora-user-images\image-20200717161102935.png)



针对与不同表的通用查询操作











## PrepareStatement

为什么要使用PrepareStatement？











解决SQL注入问题



























































































