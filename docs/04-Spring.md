# Spring

## Spring概述



在学习 Spring 框架前，先让我们回顾一下之前学习的知识。

下面是一个简单的传统 JDBC 访问数据库的代码：

```java
/*
    程序的耦合：程序间的依赖关系
        类之间的依赖
        方法之间的依赖
    解耦：不可能完全消除依赖关系，只能降低程序间的依赖关系
        实际开发中，应该做到编译期不依赖，运行时才依赖
    解耦的思路：
        第一步：使用反射来创建对象，而避免使用new关键字（新的问题：写死了，不能更改）
        第二步：通过读取配置文件的方式来获取要创建的对象全限定类名
*/
public class JdbcDemo1 {
    public static void main(String[] args) throws Exception {
        // 1. 注册驱动
//        DriverManager.registerDriver(new com.mysql.jdbc.Driver());
        Class.forName("com.mysql.jdbc.Driver");
        // 2. 获取连接
        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/eesy", "root", "123456");
        // 3. 获取操作数据库的预处理对象
        PreparedStatement pstm = conn.prepareStatement("select * from account");

        // 4. 执行sql语句，得到结果集
        ResultSet rs = pstm.executeQuery();

        // 5. 遍历结果集
        while (rs.next()) {
            System.out.println(rs.getString("name"));
        }
        // 6. 释放资源
        rs.close();
        conn.close();
        pstm.close();
    }
}
```





## Spring IoC



### 程序的耦合和解耦

#### 程序的耦合







#### 程序的解耦

解耦和思路：

第一步：使用反射来创建对象，而避免使用new关键字（新的问题：写死了，不能更改）

第二步：通过读取配置文件的方式来获取要创建的对象全限定类名





表现层调用业务层，业务层调用持久层，这就导致了程序的耦合程度很高，现在需要解决这个问题。

可以使用**工厂模式**来解决：

首先，需要一个配置文件来配置我们的service和dao，配置文件应该包含唯一标识——全限定类名；然后读取配置文件，通过反射的方式来创建对象。



```java
// 
private AccountDao accountDao = new AccountDaoImpl();

private AccountDao accountDao = (AccountDao) BeanFactory.getBean("accountDao");
```





这样就可以得到两个类的全限定类名了。





使用工厂模式解耦中存在的问题以及改造

单例模式和多例模式的区别？

单例模式只被创建一次，类中的成员只会初始化一次。

```java
com.itheima.service.impl.AccountServiceImpl@4554617c
com.itheima.service.impl.AccountServiceImpl@74a14482
com.itheima.service.impl.AccountServiceImpl@1540e19d
com.itheima.service.impl.AccountServiceImpl@677327b6
com.itheima.service.impl.AccountServiceImpl@14ae5a5
```

此时的情况是多例

多例模式的对象被创建多次，执行效率没有单例对象好







尽量不要定义类成员变量，定义为方法内部的变量。



准备一个 Map 把存储好，不必反复创建对象。

在业务层和持久层很少了类成员变量，在这种情况下，单例模式的效果会更好。

```java
public class BeanFactory {

    private static Properties props;

    // 定义一个map，用于存放我们要创建的对象，成为容器
    private static Map<String, Object> beans;

    // 使用静态代码块为 Properties 对象赋值
    // 静态代码块只初始化一次，在加载的时候就已经把map都准备好了
    static {
        try {
            // 实例化对象
            props = new Properties();
            // 获取Properties文件的流对象
            InputStream in = BeanFactory.class.getClassLoader().getResourceAsStream("bean.properties");
            props.load(in);

            // 实例化容器
            beans = new HashMap<String, Object>();
            // 取出配置文件中所有的key
            Enumeration keys = props.keys();
            // 遍历这个枚举类
            while (keys.hasMoreElements()) {
                // 取出每个key
                String key = keys.nextElement().toString();
                // 根据key获取value
                String beanPath = props.getProperty(key);
                // 反射创建对象
                Object value = Class.forName(beanPath).newInstance();
                // 把key和value存入容器中
                beans.put(key, value);
            }
        } catch (Exception e) {
            throw new ExceptionInInitializerError("初始化Properties失败");
        }
    }

    // 根据Bean的名称获取bean对象
//    public static Object getBean(String beanName) {
////        Object bean = null;
////        try {
////            String beanPath = props.getProperty(beanName);
//////            System.out.println(beanPath);
////            bean = Class.forName(beanPath).newInstance(); // 每次都会调用默认构造函数创建对象
////        } catch (Exception e) {
////            e.printStackTrace();
////        }
////        return bean;
////    }

    // 根据bean的名称获取对象，这是单例模式
    public static Object getBean(String beanName) {
        return beans.get(beanName);
    }
}
```





#### IoC（Inversion of Control）控制反转













### 使用 SpringIoC 解决程序的耦合

springioc必备jar包

```
commons-logging
beans
context
core
expression
jcl：spring把日志commons-logging集成到jcl包下了
aop：
```







学到17课

### 创建 bean 的方式

- 创建 bean 的三种方式

```xml
<!--第一种方式：使用默认构造函数创建：
在Spring配置文件中，使用bean标签配置id和class属性后，且没有其他标签时。
采用的就是默认的构造函数创建bean对象。如果该类中没有默认构造函数，则对象无法创建
-->
<bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
```



```xml
<!--第二种方式：使用普通工厂的方式创建对象（使用某个类中的方法创建对象，并存入Spring容器中-->
<bean id="instanceFactory" class="com.itheima.factory.InstanceFactory"></bean>
<bean id="accountService" factory-bean="instanceFactory" factory-method="getAccountService"></bean>
```

下面这个工厂类中有方法 `getAccountService()` ，可以通过这个方法来创建 bean 对象

```java
/*
* 模拟一个工厂类，该类可能存在与jar，我们无法修改源码的方式来提供默认构造函数
* */
public class InstanceFactory {
    public AccountService getAccountService() {
        return new AccountServiceImpl();
    }
}
```





```xml
<!--第三种方式：使用工厂中的静态方法创建对象（使用某个类中的静态方法创建对象，并存入Spring容器-->
<bean id="accountService" class="com.itheima.factory.StaticFactory" factory-method="getAccountService"></bean>
```

下面的工厂类中有静态方法 `getAccountService()` ，可以通过这个方法来创建 bean 对象

```java
/*
* 模拟一个工厂类，该类可能存在与jar，我们无法修改源码的方式来提供默认构造函数
* */
public class StaticFactory {
    public static AccountService getAccountService() {
        return new AccountServiceImpl();
    }
}
```





- bean 的作用范围

单例模式以及多例模式



```xml
<!--
    bean的作用范围调整
    bean标签的scope属性：
        作用：用于指定bean的作用范围
        取值：
            singleton：单例（默认值）
            prototype：多例
            request：作用于web应用的请求范围
            session：左右于web应用的会话范围
            global-session：作用于集群环境的会话范围（全局会话范围），但不是集群环境时候，他就是session
-->
<bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl" scope="singleton"></bean>
```



对 global-session（全局 session） 的理解：

当服务器的访问量过大，需要使用集群的方式，来降低每台服务器的负荷。

验证码一份存在表单当中，一份存在服务器的 session 域中。

![image-20200904231838216](C:\Users\Paul\Pictures\typora\Spring\Spring-01.png)







- bean 的生命周期



```xml
<!--
    bean对象的生命周期
        单例对象：
            出生：当容器创建时对象出生，解析完配置文件就创建
            活着：只要容器还在，对象一直活着
            死亡：容器销毁，对象消亡
            总结：单例对象的生命周期和容器相同
        多例对象：
            出生：当使用对象时，Spring容器为我们创建对象
            活着：对象只要还在使用过程就会一直活着
            死亡：当对象长时间不使用，且没有别的对象引用，由Java的垃圾回收器回收
-->
<bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl"
      scope="singleton" init-method="init" destroy-method="destroy"></bean>
```



以下是 Main.java 的内容

```java
public class MainApp {
    /*
        获取Spring的IoC的核心容器并根据id获取对象

        ApplicationContext的三个常用实现类
            ClassPathXmlApplicationContext：可以加载类路径下的配置文件，要求配置文件必须在类路径下。不在的话，加载不了。
            FileSystemXmlApplicationContext：可以加载磁盘任意路径下的配置文件（必须有访问权限）

            AnnotationConfigApplicationContext：用于读取注解创建容器

        核心容器的两个接口引发的问题：
        ApplicationContext：单例对象适用scope=singleton    实际开发使用多
            它在构建核心容器时，创建对象采取的策略是立即加载的方式。也就是说，一读取完配置文件马上就创建配置的对象
        BeanFactory：多例对象适用scope=prototype
            它在构建核心容器时，创建对象采取的策略是延迟加载的方式。也就是说，什么时候根据id获取对象了，什么时候才真正创建对象。
        什么时候需要立即加载？什么时候需要延迟加载？

     */
    public static void main(String[] args) {
        // 1. 获取核心容器对象，通过bean.xml配置文件
        ClassPathXmlApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");

        // 2. 根据id获取配置文件bean.xml中配置的bean对象
        // 两种方式：一种是先拿到Object类型，然后我们进行强制转换为需要的类型
        AccountService as1 = (AccountService) ac.getBean("accountService");
//        AccountService as2 = (AccountService) ac.getBean("accountService");

        // 第二种方式：传递一个字节码
//        AccountDao adao = ac.getBean("accountDao", AccountDao.class);

        System.out.println(as1);
        as1.saveAccount();

        // 手动关闭容器
        ac.close();

//        System.out.println(as2);
//        System.out.println(as1 == as2);
        /*//---------------------beanfactory---------------------//
        Resource resource = new ClassPathResource("bean.xml");
        BeanFactory beanFactory = new XmlBeanFactory(resource);
        */

    }
}
```











### Spring 的依赖注入（Dependency Injection）



```xml
<!--
    Spring的依赖注入（DI，Dependency Injection）
        IoC的作用：降低程序的耦合
        依赖关系的管理：以后都交给Spring来维护
        在当前类需要用到其他类的对象，由Spring为我们提供，我们只需要在配置文件中说明
        依赖关系的维护就称之为依赖注入。
    能够注入的数据有三类：
        基本类型和String
        其他bean类型（在配置文件或者注解中配置过的bean）
        复杂类型/集合类型
    注入的方式有三种：
        构造函数提供
        set方法提供
        使用注解提供
-->
```



- 使用构造函数注入



```xml
<!--使用构造函数注入
    使用的标签：constructor-arg
    标签出现的位置：bean标签内部；
    标签中的属性
        type：用于指定注入的数据的类型，该数据类型也是构造函数中某个或者某些参数的类型
        index：用于指定要注入的数据给构造函数中指定索引位置的参数赋值。索引位置从0开始
        name：用于指定给构造函数中指定名称的参数赋值                        常用
        ==================以上三个用于给构造函数中的参数赋值===================
        value：用于提供基本类型和String类型的数据
        ref：用于指定其他bean类型数据。他指的就是SpringIoC的核心容器中出现过的bean对象
-->
<bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
    <constructor-arg name="name" value="test"></constructor-arg>
    <constructor-arg name="age" value="18"></constructor-arg>
    <constructor-arg name="birthday" value="1970-01-01"></constructor-arg>
</bean>
```



出现的问题

```
Could not convert argument value of type [java.lang.String] to required type [java.util.Date]: Failed to convert value of type 'java.lang.String' to required type 'java.util.Date';
```



解决方法：





构造函数注入数据的方式的优点是：在获取bean对象时，注入数据是必须的操作，否则对象无法创建成功。

弊端是：改变了bean对象的实例化方式，使我们在创建对象时，用不到这些数据，也必须提供。





- set 方法注入



```xml
<!--
    set方法注入：
    涉及的标签：property
    出现的位置：bean标签内部
    标签的属性：
        name：用于指定注入时所调用的set方法名称                        常用
        value：用于提供基本类型和String类型的数据
        ref：用于指定其他bean类型数据。他指的就是SpringIoC的核心容器中出现过的bean对象
-->
<bean id="accountService2" class="com.itheima.service.impl.AccountServiceImpl2">
    <property name="name" value="test"></property>
    <property name="age" value="21"></property>
    <property name="birthday" ref="now"></property>
</bean>
```



优势：创建对象时没有明确的限制，可以直接使用默认构造函数

弊端：如果某个成员必须有值，则获取对象时有可能set方法没有执行





```xml
<!--集合/复杂类型的注入
        用于给List结构集合注入的标签：
            list array set
        用于给Map结构集合注入的标签
            map props
        结构相同，标签可以互换
-->
<bean id="accountService3" class="com.itheima.service.impl.AccountServiceImpl3">
    <property name="myStrs">
        <array>
            <value>AAA</value>
            <value>BBB</value>
            <value>CCC</value>
        </array>
    </property>

    <property name="myList">
        <array>
            <value>AAA</value>
            <value>BBB</value>
            <value>CCC</value>
        </array>
    </property>

    <property name="mySet">
        <array>
            <value>AAA</value>
            <value>BBB</value>
            <value>CCC</value>
        </array>
    </property>

    <property name="myMap">
        <map>
            <entry key="testA" value="aaa"></entry>
            <entry key="testB" value="bbb"></entry>
            <entry key="testC" value="ccc"></entry>
        </map>
    </property>

    <property name="myProps">
        <props>
            <prop key="testA">aaa</prop>
            <prop key="testB">bbb</prop>
            <prop key="testC">ccc</prop>
        </props>
    </property>
</bean>
```













### 基于注解的 IoC 配置

在类中添加一些注解实现 IoC



@Component

@Controller

@Service

@Repository





涉及属性注入的注解：

@Autowired：**自动按照类型注入**

使用注解方式注入的时候，set 方法就不是必须的了。



```java
@Component()
public void 
```



Spring 容器是一个 Map 结构，

![image-20200907214249220](C:\Users\Paul\Pictures\typora\Spring\Autowired自动按照类型注入.png)



当实现类不是唯一的时候，会报错

首先会根据数据类型找到相应的范围，然后会根据变量名称找id。





上面的解决方式显然不是我们想要的。

@Qualifier







@Resource



总结：以上三个注解都只能注入其他bean类型的数据，而基本类型和String类型无法使用上述注解实现。另外，集合类型的注入只能通过XML的方式实现





基于 XML 的 IoC 小案例

创建一个 maven 工程，在 pom.xml 文件中引入依赖

```xml
<dependencies>
	<dependency>
    	<
    </dependency>
</dependencies>
```









首先创建一个实体类对象 Account

```java
public class Account {
    private Integer id;
    private String name;
    private Float money;
    
    /*
    setter and getter method
    */
}
```



然后





基于注解的 IoC 小案例





## Spring AOP



什么是 AOP？





为什么需要 AOP？



正常执行 SQL 的步骤如下：

1. 打开数据库连接池获得数据库连接资源
2. 执行相应的 SQL 语句，对数据进行操作
3. 如果 SQL 执行过程发生异常，回滚事务
4. 如果 SQL 执行阶段没有发生异常，最后提交事务
5. 到最后的阶段，需要关闭一些连接资源



如果是 Spring AOP

可以按照如下的方式实现：

1. 打开数据库连接在 before 方法中完成
2. 执行 SQL，按照读者的逻辑会采取反射的机制调用
3. 如果发生异常，则回滚事务；如果没有发生异常，则提交事务，然后关闭数据库连接资源







AOP 的术语



- 切面





- 通知

前置通知



后置通知





返回通知





异常通知



环绕通知



- 引入







- 切点





- 连接点







- 织入

生成动态代理的过程。



![image-20200907200011629](C:\Users\Paul\Pictures\typora\Spring\spring-03.png)



Spring 并不是 Spring 特有的，Spring 只是支持

