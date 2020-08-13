# SpringMVC

服务器端分为三层架构

表现层SpringMVC

业务层Spring

持久层MyBatis









B/S架构



C/S架构



## 1. 概述

Spring为展现层提供的鲫鱼MVC设计理念的Web框架，是目前最主流的MVC框架之一。



**SpringMVC 通过一套MVC注解，让 POJO 成为处理请求的控制器，无需实现任何接口。**



支持 RESTful 风格的URL请求



采用松耦合可插拔组件结构，比其他MVC框架更具扩展性和灵活性。



清晰的角色划分：

前端控制器（DispatcherServlet）

请求到处理器映射（HandlerMapping）

处理器适配器

视图解析器

处理器或者页面控制器（Controller）

验证器

命令对象

表单对象







DispatcherServlet 就是**前端控制器**，任务就是将请求发送给 SpringMVC 控制器（controller）。控制器是一个用于处理请求的 Spring 组件，在典型的应用程序中会有多个控制器，DispatcherServlet 需要知道应该将请求发送给哪个控制器。所以 DispatcherServlet 会查询一个或者多个**处理器映射**（handler mapping）来确定请求的下一站，处理映射会根据请求所携带的 URL 信息进行决策。



控制器处理信息（实际上，设计良好的控制器只处理很少甚至不处理工作，而是将业务逻辑委托给一个或者多个服务对象进行处理）



控制器在完成逻辑处理后，通常会产生一些信息，这些信息需要被返回给用户并且在浏览器上显示。这些信息被称为模型（model）。不过，仅仅是返回原始的信息是不够的的——这些信息需要被良好的方式进行格式化，一般是 HTML。所以，信息需要发送一个视图（view），通常会是 JSP。



控制器做的最后一件事情设是将模型数据打包，并且标示出用于渲染的视图名。接下来会将请求连同模型和视图名发送回 DispatcherServlet。



DispatcherServlet 会使用**视图解析器（view resolver）**来将逻辑视图名匹配为一个特定的视图实现，不一定是 JSP，也可以是 html。







## 2. SpringMVC 入门案例

入门程序：

点击超链接发送请求，编写一个类接受到，转发成功到新的jsp页面。

1. 需要搭建开发的环境
   1. 

1. 编写入门的程序

```
1. 启动服务器，加载一些配置文件
	dispatcherServlet
	springmvc.xml被加载了
	HelloController创建成对象
	视图解析器变为对象
	

2. 发送请求，后台处理请求
	任何请求都会经过前端控制器dispatcherServlet
```



![image-20200803230653034](https://tva1.sinaimg.cn/large/007S8ZIlly1ghe0vzlqn6j30p20aeq4o.jpg)



![springmvc执行流程原理](https://tva1.sinaimg.cn/large/007S8ZIlly1ghe1hm7t9xj30o00dxjtf.jpg)



```xml
<!--开启SpringMVC框架注解的支持-->
<mvc:annotation-driven/>
```

在 SpringMVC 的各个组件中，**处理器映射器、处理器适配器、视图解析器**成为 SpringMVC 的三大组件。

上述配置可以替代处理器和适配器的配置。



@RequestMapping 注解

用于建立请求 URL 和处理请求方法的关联

```java
@Controller
public class HelloController {
    @RequestMapping(value="hello", method=GET) // 处理对“hello”的GET请求
    public String sayHello() {
        return "success" // 视图名称为 success
    }
}
```



第一个属性

path和value，路径



第二个属性

method，接受的请求的方式

学习枚举类



```java
@RequestMapping(value="testRequestMapping", method = {RequstMethod.POST, RequstMethod.GET})
```

RequstMethod 是一个枚举类

```java
public enum RequestMethod {

	GET, HEAD, POST, PUT, PATCH, DELETE, OPTIONS, TRACE

}
```

第三个属性

params















## 3. 请求参数的绑定









将数据封装到 JavaBean 中

首先定义一个 JavaBean 来封装数据。

```java
package cn.itcast.domain;

import java.io.Serializable;

public class Account implements Serializable {
    private String username;
    private String password;
    private double money;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public double getMoney() {
        return money;
    }

    public void setMoney(double money) {
        this.money = money;
    }

    @Override
    public String toString() {
        return "Account{" +
                "username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", money=" + money +
                '}';
    }
}
```



在 index.jsp 中定义表单

```html
<form action="/param/saveAccount" method="post">
    姓名：<input type="text" name="username" /><br/>
    密码：<input type="text" name="password" /><br/>
    金额：<input type="text" name="money" /><br/>
    <input type="submit" value="提交" />
</form>
```



```java
/*
* 请求参数绑定，把数据封装到JavaBean类中
* */
@RequestMapping("/saveAccount")
public String saveAccount(Account account) {
    System.out.println(account);
    return "success";
}
```



进阶：

如果需要封装的数据中包含**引用数据类型**





中文乱码问题：

POST请求下中文会乱码

GET请求不会

SpringMVC 框架提供了过滤器，可以解决字符串乱码的问题

```xml
<!--配置解决中文乱码的过滤器-->
<filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```





类中有集合的情况怎么封装数据

把数据封装进Account类中，类中存在List和Map集合

```xml
<%--把数据封装到类中，类中存在集合--%>
    <form action="/param/saveAccount" method="post">
        姓名：<input type="text" name="username" /><br/>
        密码：<input type="text" name="password" /><br/>
        金额：<input type="text" name="money" /><br/>

        用户姓名：<input type="text" name="list[0].uname" /><br/>
        用户年龄：<input type="text" name="list[0].age" /><br/>

        用户姓名：<input type="text" name="map['one'].uname" /><br/>
        用户年龄：<input type="text" name="map['one'].age" /><br/>
        <input type="submit" value="提交" />
    </form>
```



测试 Servlet 原生的 API

```java
@RequestMapping("/testServlet")
public String testServlet(HttpServletRequest request, HttpServletResponse response) {
    System.out.println("执行了");
    System.out.println(request);
    HttpSession session = request.getSession();
    System.out.println(session);
    ServletContext servletContext = session.getServletContext();
    System.out.println(servletContext);
    System.out.println(response);
    return "success";
}
```



web.xml文件

```xml
<a href="param/testServlet">Servlet的原生API</a>
```







### 3.2 特殊情况

自定义类型转换器

日期转换的时候出错了















## 4. 常见的注解





### 4.1 RequestParam



```jsp
<a href="anno/testRequestParam?username=哈哈">RequestParam</a>
```



```java
@Controller
@RequestMapping("/anno")
public class AnnoController {
    @RequestMapping("/testRequestParam")
    public String testRequestParam(@RequestParam(name="name") String username) {
        System.out.println(username);
        return "success";
    }
}
```

获取 username 参数。



但是如果 username 换成了 name，提交数据的 key 与方法参数的名称不一致。这时候数据就会封装不上。



解决方案：

修改绑定规则，这个时候就需要使用 @RequestParam 这个注解。

```java
@Controller
@RequestMapping("/anno")
public class AnnoController {
    @RequestMapping("/testRequestParam")
    // 之前传入的参数是什么，这里的name就写什么，把name赋值给username
    public String testRequestParam(@RequestParam(name="name") String username) {
        System.out.println(username);
        return "success";
    }
}
```



源码

require 属性







### 4.2 RequestBody



获取请求体内容。直接使用



```xml
<form action="/anno/testRequestBody" method="post">
    用户姓名：<input type="text" name="username" /><br/>
    用户年龄：<input type="text" name="age" /><br/>
    <input type="submit" value="提交" />
</form>
```

获取表单的数据



```java
/*
* 获取请求体的内容
* */
@RequestMapping("/testRequestBody")
public String testRequestBody(@RequestBody String body) {
System.out.println("执行了...");
System.out.println(body);
return "success";
}
```



异步的时候需要这个注解





需要传递 JSON 数据的时候





接受列表数据和表单序列化









### 4.3 PathVariable



使用 URL 传递参数

用于绑定 url 中的占位符，例如，请求 url 中 /delete/{id}，这个{id}就是 url 占位符。

RESTFul 编程风格优点

结构清晰、易于理解、扩展方便



请求地址都一样，可以根据请求方式来判断执行哪个方法



![image-20200806000246103](https://tva1.sinaimg.cn/large/007S8ZIlly1ghgdqri3zuj30fq081wez.jpg)











### 4.4 RequestHeader



用于获取请求消息头











### 4.5 CookieValue









### 4.6 ModelAttribute



提交表单数据不完整

 







### 4.7 SessionAttribute



我们需要把数据暂时保存到 HTTP 的 request 对象或者 Session 对象中；在开发控制器的时候有时候也需要把数据保存到对象中；或者从中获取数据。





@RequestAttribute







@SessionAttribute









@SessionAttributes









## 第二章



### 响应数据和结果视图



testString







testVoid

如果使用不了视图解析器的话，需要自己配置路径地址







testModelAndView





转发和重定向

```html
<a href="user/testForwardOrRedirect">testForwardOrRedirect</a>
```



```java
 /*
  * 使用关键字的方式进行转发和重定向
  * */
@RequestMapping("/testForwardOrRedirect")
public String testForwardOrRedirect() {
    System.out.println("testForwardOrRedirect方法执行了...");
    // 请求的转发
    //        return "forward:/WEB-INF/pages/success.jsp";

    // 重定向
    return "redirect:/index.jsp";
}
```







页面发送一个 ajax 请求，是一个异步请求，后台需要把一些对象转换为 json 格式的字符串响应回去。

@ResponseBody 响应 json 数据

dispatcherServlet 会拦截所有的请求，导致静态资源（img、css、js）也会被拦截，从而不能使用。解决办法就是配置静态资源不被拦截，在 springmvc.xml 中配置

```xml

```







学习到08课



需要告诉前端控制器静态页面不要拦截







### SpringMVC 实现文件上传











### SpringMVC中的异常处理











### SpringMVC的拦截器

























































