# Spring框架学习

## Spring的Bean管理

### xml方式









### 注解方式

如何注解定义Bean

@Component：描述Spring框架中Bean

@Repository：用于对DAO实现类进行标注

@Service：用于对Service实现类进行标注

@Controller：用于对Controller实现类进行标注

这三个注解是为了标注类本身的用途更加清晰







## 属性注入的方式



### xml方式





属性注入的方法



构造方法属性注入

```java
public class SpringDemo4 {
    @Test
    public void demo1() {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        User user = (User)applicationContext.getBean("user");
        System.out.println(user);
    }
}
```



xml文件

```xml
<bean id="user" class="com.imooc.ioc.demo4.User">
	<constructor-arg name="name" value="张三"/>
	<constructor-arg name="age" value="23"/>
</bean>
```





setter方法属性注入

```xml
<!--setter方法属性注入-->
<bean id="person" class="com.imooc.ioc.demo4.Person">
	<property name="name" value="李四"/>
	<property name="age" value="32"/>
</bean>
```

普通类型的值使用value，对象类型的值使用ref。



p名称空间

```xml
p:<属性名>="xxx" 引入常量值
p:<>-ref="xxx"引用其他Bean对象
```





SpEL注入

spring expression language，spring表达式语言，对依赖注入进行简化

```
SpEL表达式语言
语法：#{}
#{'hello'}：使用字符串
#{beanId}：使用另一个bean
#{beanId.content.toUpperCase()}：使用指定名属性，并使用方法
#{T(java.lang.Math).PI}：使用静态字段或者方法
```





复杂类型的属性注入

比如数组，集合等，主要用在Spring整合其他的框架时，框架里定义了Bean等

- 数组类型的属性注入



- List集合类型的属性注入





- Set集合类型的属性注入





- Map集合类型的属性注入





- Properties类型的属性注入





### 注解方式









## 传统XML配置和注解混合使用

- XML方式的优势
  - 结构清晰，易于阅读
- 注解方式的优势
  - 开发便捷，属性注入方便



- XML与注解的整合开发

1. 引入context命名空间
2. 在配置文件中添加context:annotation-config标签























## Spring 的 AOP



什么是 AOP？

面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。

采用横向抽取机制，取代了传统从继承体系的重复性代码（性能监控，事务管理，安全检查和缓存）

AOP 使用纯 Java 代码实现，不需要专门的编译过程和类加载器，在运行期间通过代理方式向目标类织入增强代码。





```
AOP

public class UserDao {
	public void save() {
		// 保存用户
	}
	
}
```

需要在保存用户之前进行权限校验，必须是管理员才能操作。



纵向继承

```java
public class BaseDaoImpl {
    public void checkPrivilege() {
        //
    }
}

public class UserDaoImpl extends BaseDaoImpl {
    public void save(User user) {
        checkPrivilege();
        // 保存用户
        
    }
    ...
}
```

使用AOP来解决这些问题，采用横向抽取机制（代理机制）来取代传统方式的继承。

```java
public class UserDaoImpl implements UserDao {
    public void save(User user) {
        // 保存用户
    }
}
```

使用 JDK 的动态代理对实现了 UserDao 的类产生一个代理对象。在代理对象中对 save 方法进行增强。



### AOP的相关术语

| 相关术语             | 解释                                                         |
| -------------------- | ------------------------------------------------------------ |
| Joinpoint（连接点）  | 就是那些被拦截的点。在Spring中，这些点指的是方法，因为Spring只支持方法类型的连接点 |
| Pointcut（切入点）   | 对哪些Jointpoint进行拦截的定义                               |
| Advice（通知/增强）  | 拦截到Joinpoint之后要做的事情就是通知，分为前置通知，后置通知，异常通知，最终通知，环绕通知（切面要完成的功能） |
| Introduction（引介） | 一种特殊的通知，在不修改类代码的前提下，可以在运行期为类动态地添加一些方法或者Field（不需要管类层面的增强） |
| Target（目标对象）   | 代理的目标对象                                               |
| Weaving（织入）      | 指的是把增强应用到目标对象来创建新的代理对象的过程。Spring采用动态代理织入，而AspectJ采用编译期织入和类装载期织入 |
| Proxy（代理）        | 一个类被AOP织入增强后，就产生一个结果代理类                  |
| Aspect（切面）       | 切入点和通知（引介）的结合                                   |

```java
public class UserDaoImpl implements UserDao {
    public void save(User user) {
        
    }
    
    public void update(User user) {
        
    }
    
    public List find() {
        
    }
    
    public void delete(User user) {
        
    }
}
```



增删改查这些方法都可以被增强，这些方法称为连接点。

只想对save方法进行增强（做权限校验），save方法被称为切入点。

对save方法进行权限校验，权限校验的方法称为通知。

UserDaoImpl就是要增强的对象，Target

将Advice应用到Target的过程就叫做织入。（将权限校验应用到UserDaoImpl的save方法的过程）

Proxy（代理）：被应用了增强后，产生一个代理对象。



### AOP 的底层实现

JDK动态代理实现

```java
public class MyJdkProxy {
    public Object createProxy() {
        
    }
}
```









CGLIB实现动态代理









代理知识总结

Spring在运行期，生成动态代理对象，不需要特殊的编译器

Spring AOP 的底层就是通过 JDK 动态代理或者 CGLib 动态代理技术为目标 Bean 执行横向织入

1. 若目标对象实现了若干接口，Spring使用 JDK 的 java.lang.reflect.Proxy 类代理。
2. 若目标对象没有实现任何接口，Spring使用 CGLib 库生成目标对象的子类。



程序中应优先对接口创建代理，便于程序解耦合。

标记为final的方法，不能被代理，因为无法进行覆盖。

- JDK 动态代理，是针对接口生成子类，接口种方法不能使用 final 修饰。
- CGLib 是针对目标类产生子类，因此类或者方法不能使用 final 修饰。

Spring 只支持方法连接点，不提供属性连接点。



Spring AOP 增强类型

AOP 联盟为通知 Advice 定义了 org.aopalliance.Interface.Advice

Spring 按照通知 Advice 在目标类方法的连接点位置，可以分为5类

- 前置通知org.springframework.aop.MethodBeforeAdvice
  - 在目标方法执行前实施增强
- 后置通知org.springframework.aop.AfterReturningAdvice
  - 在目标方法执行后实施增强

- 环绕通知org.springframework.aop.MethodIntercepter
  - 在目标方法执行前后实施增强
- 异常抛出通知org.springframework.aop.ThrowsAdvice
  - 在方法抛出异常后实施增强



Spring AOP 切面类型

Advisor：代表一般切面，Advisor 本身就是一个切面，对目标类所有方法进行拦截

PointcutAdvisor：代表具有切面的切面，可以指定拦截目标类哪些方法。

IntroductionAdvisor：



不需要写 JDK 动态代理或者 CGLib 动态代理的代码了。







### Spring 对 AOP 的支持



Advisor 切面案例



使用普通Advice作为切面，会对目标类中的所有方法都进行拦截（增强），不够灵活，在实际开发过程中常用带有切点的切面。



PointcutAdvice 切点切面

常用的 PointcutAdvice 实现类

- DefaultPointcutAdvice 最常用的切面类型它可以通过任意的Pointcut和Advice组合定义切面
- JdkRegexpMethodPointcut 构造正则表达式切点





## 使用AspectJ 注解开发 Spring AOP

灵活，方便操作。



提供的不同的通知类型

@Before 前置通知，相当于 BeforeAdvice

@AfterReturning 后置通知，相当于 AfterReturningAdvice

@Around 环绕通知，相当于 MethodInterceptor

@AfterThrowing 异常抛出通知，相当于 ThrowAdvice

@After 最终 final 通知，不管是否异常，该通知都会执行

@DeclareParents 引介通知，相当于



在通知中通过 value 属性定义切点

通过 execution 函数，可以定义切点的方法切入

语法：

```
execution(<访问修饰符>?<返回类型><方法名>(<参数>)<异常>)

# 匹配所有类的 public 方法
execution(public * * (..))
# 匹配指定包下的所有类方法
execution(* com.imooc.dao.* (..)) # 不包含子包
execution(* com.imooc.dao..* (..)) # 包含子包

# 匹配指定类所有方法
execution(* com.imooc.service.UserService.* (..))

# 匹配实现特定接口所有类方法
execution(* com.imooc.dao.GenericDAO+.*(..))

# 匹配所有 save 开头的方法
execution(* save* (..))
```



为目标类，定义切面类

MyAspectAnno.java

```java
@Aspect
public class MyAspectAnno() {
    @Before(execution())
}
```





### @Before 前置通知

可以在方法中传入 JoinPoint 对象，用来获得切点信息

```java
// 要增强的代码
@Before(value="execution(* com.imooc.aspectJ.demo1.ProductDao.save(..))")
public void before(JoinPoint joinPoint) {
    System.out.println("===========前置通知===========" + joinPoint);
}
```



### @AfteReturning 后置通知

通过 returning 属性可以定义返回值，作为参数

```java
@AfterReturning(value="execution(* com.imooc.aspectJ.demo1.ProductDao.update(..))", returning = "result")
public void afterReturning(Object result) {
    System.out.println("======后置通知=====" + result);
}
```





### @Around 环绕通知

around 方法的返回值就是目标代理方法执行返回值

参数为 ProceedingJoinPoint 可以调用拦截目标方法执行

```java
// 环绕通知
@Around(value="execution(* com.imooc.aspectJ.demo1.ProductDao.delete(..))")
public Object around(ProceedingJoinPoint joinPoint) throws Throwable {
    System.out.println("环绕前通知");
    Object obj = joinPoint.proceed(); // 执行目标方法
    System.out.println("环绕后通知");
    return obj;
}
```

如果不调用 ProceedingJoinPoint 的 proceed 方法，那么目标函数就被拦截了。



### @AfterThrowing 异常抛出通知

通过设置 throwing 属性，可以设置发生异常对象参数

```java
// 
@AfterThrowing(value="execution(* com.imooc.aspectJ.demo1.ProductDao.findAll(..))", throwing = "e")
public void afterThrowing(Throwable e) {
    System.out.println("======异常通知=====" + e);
}
```



### @After 最终通知

无论是否有异常，最终通知都会被执行

```java
// 最终通知
@After(value="execution(* com.imooc.aspectJ.demo1.ProductDao.findOne(..))")
public void after() {
    System.out.println("=====最终通知=====");
}
```





### 通过 @Pointcut 为切点命名

在每个通知内定义切点，会造成工作量大，不易维护，对于重复的切点，可以使用 @Pointcut 进行定义。

切点方法：private void 无参数方法，方法名为切点名

当通知多个切点时，可以使用||进行连接

```java
@Pointcut(value="execution(* com.imooc.aspectJ.demo1.ProductDao.save(..))")
public void myPointcut() {}
```



```java
@Before(value="myPointcut()")
public void before(JoinPoint joinPoint) {
    System.out.println("===========前置通知===========" + joinPoint);
}
```











## 使用 XML 配置开发 Spring AOP

使用 XML 配置切面

第一步：引入 jar 包，spring

第二步：创建一个配置文件



定义一个切面类

```java

```









配置 AOP 的增强

```xml
<!--XML的配置方式完成AOP的开发-->
<!--配置目标类-->
<bean id="customerDao" class="com.imooc.aspectJ.demo2.CustomerDaoImpl"/>

<!--配置切面类-->
<bean id="myAspectXml" class="com.imooc.aspectJ.demo2.MyAspectXml"/>

<aop:config>
    
    <!--配置切入点：哪些方法需要增强-->
    <aop:pointcut id="pointcut1" expression="execution(* com.imooc.aspectJ..demo2.CustomerDao.save(..))"/>
    <aop:pointcut id="pointcut2" expression="execution(* com.imooc.aspectJ..demo2.CustomerDao.update(..))"/>
    <aop:pointcut id="pointcut3" expression="execution(* com.imooc.aspectJ..demo2.CustomerDao.delete(..))"/>
    <aop:pointcut id="pointcut4" expression="execution(* com.imooc.aspectJ..demo2.CustomerDao.findOne(..))"/>
    <aop:pointcut id="pointcut5" expression="execution(* com.imooc.aspectJ..demo2.CustomerDao.findAll(..))"/>
    
    <!--配置AOP的切面-->
    <aop:aspect ref="myAspectXml">
        <!--配置前置通知-->
        <aop:before method="before" pointcut-ref="pointcut1"/>
        <!--配置后置通知-->
        <aop:after-returning method="afterReturning" pointcut-ref="pointcut2"/>
        <!--配置环绕通知-->
        <aop:around method="around" pointcut-ref="pointcut3"/>
        <!--配置异常通知-->
        <aop:after-throwing method="afterThrowing" pointcut-ref="pointcut4"/>
        <!--配置最终通知-->
        <aop:after method="after" pointcut-ref="pointcut5"/>
    </aop:aspect>
    
</aop:config>
```















## JDBC Template

使用 Spring 组件 JDBC Template 简化持久化操作

为了简化持久化操作，Spring 在 JDBC API 之上提供了 JDBC Template 组件。

程序员代码→JDBC Template→JDBC API→JDBC 驱动→数据库

不直接操作 JDBC API，代码比较繁琐。

在保留代码灵活性的基础上，尽量减少持久化代码。







需要提前掌握的知识：

JDBC

Spring IoC

Spring AOP

MySQL





概念





环境配置

引入 jar 包



配置数据库

```xml
<!--dataSource配置-->
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/selection_course?useUnicode=true&amp;&amp;characterEncoding=utf-8"/>
    <property name="username" value="root"/>
    <property name="password" value="123456"/>
</bean>

<!--通过这个id可以得到实例-->
<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <!--注入属性-->
    <property name="dataSource" ref="dataSource"/>
</bean>
```









基本操作

execute 方法

```java
@Test
public void testExecute() {
    sql = "";
    jdbcTemplate.execute(sql);
}
```





update 与 batchUpdate 方法

update 方法对数据进行增删改查操作

```java
int update(String sql, Object[] args);
int update(String sql, Object...args);
```



batchUpdate 方法批量增删改操作

```java
int[] batchUpdate(String[] sql);
int[] batchUpdate(String sql, List<Object[]> args);
```







查询方法

query 与 queryXXX 方法

获取一个

```
T queryForObject(String sql, Class<T> type)
T queryForObject(String sql, Object[] args, Class<T> type)
T queryForObject(String sql, Class<T> type, Object...args)
```



获取多个

```
List<T> queryForList(String sql, Class<T> type)
List<T> queryForList(String sql, Object[] args, Class<T> type)
List<T> queryForList(String sql, Class<T> type, Object...args)
```





call 方法





持久层









优缺点分析













## Spring 事务管理

### Java 事务导引

什么是事务？

事务是正确执行一系列的操作（或动作），使得数据库从一种状态转换为另一种状态，并且保证操作全部成功，或者全部失败。





事务的原则性内容

必须服从 ISO/IEC 所制定的 ACID 原则

ACID 原则具体内涵如下：

- 原子性（Atomicity）：即不可分割，事务要么全部执行，要不就全部不被执行
- 一致性（Consistency）：事务的执行使得数据库从一种正确状态转换成另一种正确状态
- 隔离性（Isolation）：在事务正确提交之前，它可能的结果不应显示给任何其他事务。
- 持久性（Durability）：事务正确提交后，其结果将永久保存在数据库中。

为了保证数据操作的安全性。



事务和 Java 的关系

Java 事务的产生，程序操作数据库的需要。在 Java 编写的程序或者系统中，实现 ACID 的操作。

事务实现的范围

- 通过 JDBC 相应方法间接来实现对数据库的**增删改查**，把事务转移到 Java 程序代码中进行控制。
- 确保事务要么全部成功，要么撤销不执行。



事务的实现方式

JDBC 事务：用 Connection 对象控制，包括手动模式和自动模式。

JTA（Java Transation API）：与实现无关的，与协议无关的 API。

容器事务：应用服务器提供的，且大多是基于 JTA 完成（通常基于 JNDI 的，相当复杂的 API 实现）



三种事务的差异：

JDBC事务：控制的局限性在一个数据库连接内，但是其使用简单。

JTA（Java Transation API）：功能强大，可阔约多个数据库或者多 DAO，使用比较复杂。

容器事务：主要指的是 J2EE 应用服务器提供的事务管理，局限与 EJB





### Spring 事务核心接口



三个核心接口

事务定义接口

事务管理接口

事务状态接口

![事务接口架构](C:\Users\Paul\Pictures\Java\事务接口架构.png)



JDBC





Spring 事务属性

事务属性范围：

- 传播行为

- 隔离规则

- 回滚规则

- 事务超时

- 是否只读？

通过下面这个接口实现事务属性的定义

```java
public interface TransactionDefinition {
    // 返回事务的传播行为
    int getPropagationBehavior();
    // 返回事务的隔离级别，事务管理器根据它来控制另外一个事务可以看到本事务内的哪些数据
    int getIsolationLevel();
    // 返回事务必须在多少秒内完成
    int getTimeout();
    // 这个返回值进行优化，确保事务是只读的
    boolean isReadOnly();
}
```



事务读取类型：

脏读：事务没提交，提前读取

不可重复读：两次读取的数据不一致

![不可重复读](C:\Users\Paul\Pictures\Java\不可重复读.png)

幻读：事务不是独立执行时发生的一种非预期现象

比如：事务1在修改数据，另一个事务在增加数据

![幻读](C:\Users\Paul\Pictures\Java\幻读.png)



事务隔离级别

定义了一个事务可能受其他并发事务影响的程度

![事务隔离级别](https://raw.githubusercontent.com/paul0316/picgo/master/img/%E4%BA%8B%E5%8A%A1%E9%9A%94%E7%A6%BB%E7%BA%A7%E5%88%AB.png)







### 编程式事务管理









### 事务最佳实践





































































































