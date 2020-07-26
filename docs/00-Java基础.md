# Java基础

> 这里的内容比较基础，但也是十分重要的，基础要打牢！







## 数据类型与运算符

## 2.4 常量和变量

### 常数

常量表示不能改变的数值

Java中常量的分类：

1. 整数常量
2. 小数常量
3. 布尔值
4. 字符常量(单引号'')
5. 字符串常量(双引号"")
6. null常量

对于整数，有四种表现形式：

1. 二进制
2. 八进制：0-7，满8进1，用0开头
3. 十进制
4. 十六进制：0-9，A-F，满16进1，用0x开头



数据的最小单元——bit位

1代表关，0代表开

1byte 字节 = 8个二进制位（bit位）

1k = 1024bytes

0100-1011

三个bit位为一组

### 进制转换

- 正数的二进制表现形式
  - 十进制----->二进制
    - 除以2取余数
  - 二进制----->十进制
    - 乘以2的幂数

对于八进制数，其实就是三个二进制位一个八进制位

对于十六进制，其实就是四个二进制位一个十六进制



- 负数的二进制表现形式

其实就是这个数的相反数二进制取反，加1

```python
 0001-0110----->6
 1110-1001----->6二进制数取反
+0000-0001----->加1
----------
 1110-1010----->-6二进制数
```

负数的二进制的最高位是1



### 变量

- 变量的概念
  - 内存中的一个存储区域
  - 该区域有自己的名称（变量名）和类型（数据类型）
  - 该区域的数据可以在同一类型范围内不断变化
- 为什么要定义变量
  - 用来不断存放同一类型的常量，并且可以重复利用
- 使用变量注意：
  - 变量的作用范围（一对{}之间有效）
  - 初始化值
- 定义变量的格式
  - 数据类型 变量名 = 初始化值:
  - 注：格式是固定的，记住格式，以不变应万变

```java
class VarDemo
{
    public static void main(String[] args)
    {
        //数据类型 变量名 = 初始化值
        byte b = 3;
        //byte b = 300 报错：可能损失精度
        short s = 4000;
        int x = 12;
        long l = 123437242l;  //非常大的数后面需要加个l
        
        float f = 2.3f;
        double d = 3.4;
        
        char ch = 'a';
        
        boolean bl = true;
        bl = false;
            
            System.out.println(b)
    }
}
```

Java语言是强类型语言，对于每一种数据都定义了明确的具体数据类型，在内存中分配了不同大小的内存空间

数值型byte, short, int, long

浮点数型float, double

字符型char 2个字节

布尔型boolean

类class

接口interface

数组[]

整数默认：int

小数默认：double（双精度）

有些时候没有必要用很大的空间去存储小的数据，节省空间

- 作用域

### 自动类型转换和强制类型转换

```java
class VarDemo2
{
    public static void main(String[] args)
    {
        //int x = 3;
        //byte b = 5;
        //x = x + b;  //自动类型转换
        byte b = 3;
        b = b + 4;
        b = (byte)(b + 4);  //强制类型转换，但是这种方式会丢失高位数据
        System.out.println(b);
    }
}
```

类型转换的原理？



什么时候要用强制类型转换？



表达式的数据类型自动提升

- 所有的byte型、short型和char的值都将被提升为int型
- 如果一个操作数是long型，计算结果就是long型
- 如果一个操作数是float型，计算结果就是float型
- 如果一个操作数是double型，计算结果就是double型



分析：System.out.println('a') 和 System.out.println('a'+1)的区别？

```java
class VarDemo
{
    public static void main(String[] args)
    {
        System.out.println('a');
        System.out.println('a' + 1);
        System.out.println((char)('a' + 1));
        System.out.println('你');
        System.out.println('你' + 0);
    }
}
```



- 计算机如何识别我们的文字呢？

二进制与生活文字的对应关系表——编码表

ASCII——American Standard Code for Information Interchange

GB2312——

GBK

- Java语言里内置了Unicode国际标准表

Unicode（[统一码](https://baike.baidu.com/item/统一码)、万国码、单一码）是计算机科学领域里的一项业界标准，包括字符集、编码方案等。Unicode 是为了解决传统的字符编码方案的局限而产生的，它为每种语言中的每个字符设定了统一并且唯一的[二进制](https://baike.baidu.com/item/二进制)编码，以满足跨语言、跨平台进行文本转换、处理的要求。

```java
class VarDemo2
{
    public static void main(String[] args)
    {
        //int x = 3;
        //byte b = 5;
        //x = x + b;  //自动类型转换
        //byte b = 3;
        //b = b + 4;
        //b = (byte)(b + 4);  //强制类型转换，但是这种方式会丢失高位数据
        //System.out.println(b);
        
        byte b = 4;
        //b = 3 + 7;
        byte b1 = 3;
        byte b2 = 7;
        b = b1 + b2;  //b1和b2是变量，不确定，无法进行赋值；若b2=127，则不行
        
        int x;
        //x = b1 + b2;  //无论b1和b2多大，int类型都可以装的下，所以可以执行
        int x1 = Integer.MAX_VALUE;
        int x2 = 2;
        x = x1 + x2;
        
        System.out.println(x)
    }
}
```





### 算术运算符

```java
class OperatorDemo
{
    public static void main(String[] args)
    {
        //算术运算符    + = * / (%取余数，模运算) (+连接符)
        int x = 6370;
        x = x / 1000 * 1000;
        System.out.println(x);  //输出6000
        System.out.println(5%2);
        System.out.println(3 + "2");  //这里的+就是连接符
        System.out.println("5 + 5 = " + 5 + 5);  //"5+5=5"+5--->"5+5=55"
        System.out.println("5 + 5 = " + (5 + 5);  //5+5=10
        int a = 4, b = 5;
        System.out.println("a = " + a + ", b = " + b);  //a = 4, b =5
    }
}
```

任何数%2不是0就是1，可以应用于各种开关算法

```java
class OperatorDemo2
{
    public static void main(String[] args)
    {
        //自增
        int a = 3, b;
        b = a++;
        b = ++a;
        System.out.println("a = " + a + ", b = " + b);
        
    }
}
```

**a++和++a的区别：**

a ++ 运算符在操作数之后，称为**“后增量”**。a变量自增，返回自增之前的值;

++ a 运算符在操作数之前，称为**“前增量”**。a变量自增，返回自增之后的值。

进阶

```java
class OperatorDemo3
{
    public static void main(String[] args)
    {
        //自增
        int i = 3;
        i = i++;  //
        System.out.println("i = " + i);
        
    }
}
```



### 赋值运算符

```java
class OperatorDemo4
{
    public static void main(String[] args)
    {
        //赋值运算符 =, +=, -=, *=, /=, %=
        short s = 3;
        s += 4;
        s = s + 4;
        System.out.println("s = " + s);
        
    }
}
```

面试题

初始化 `short s = 3` ，请问`s += 4` 和 `s = s + 4`的区别?

前一个赋值，先检查，然后自动强制转换

后一个不做自动转换，必须用 `s = (short)(s + 4)`



### 比较运算符

比较运算符运算完都有结果，不是true就是false

```java
class OperatorDemo5
{
    public static void main(String[] args)
    {
        //比较运算符，运算完的结果不是true就是false
        System.out.println(3>2);  // true
        System.out.println(3==2);  //false
        System.out.println(3!=2);  //true
    }
}
```



### 逻辑运算符

```java
class OperatorDemo6
{
    public static void main(String[] args)
    {
        /*逻辑运算符，用于连接boolean类型的表达式
        &：与
        |：或
        ^：异或
        
        &运算特点：
        true & true = false
        true & false = false
        false & true = false
        false & false = true
        &运算规律：
        两边都为true，结果才为true
        
        |运算特点：
        true | true = false
        true | false = false
        false | true = false
        false | false = true
        |运算规律：
        两边只要有一个true，结果就为true
        
        ^异或：和或有点不一样
        ^运算特点：
        true ^ true = false
        true ^ false = true
        false ^ true = true
        false ^ false = false
        ^运算规律：
        两边符号相同，结果为false；两边符号不同时，结果为true
        
        ！：非运算
        !true = false;
        !false = true;
        !!true = true;
        
        面试题
        &与&&的区别：
        &两边一定都参与运算
        &&一旦左边为false的情况下，右边不进行运算，结果就为false
        
        |与||的区别：
        |两边一定都参与运算
        ||一旦左边为true的情况下，右边不进行运算，结果就为true
        
        */
        
        int x = 3;
        System.out.println(x > 2 & x < 5);  //true
    }
}
```

**注意：**

Java语言里面的异或与

### 位运算符（用于二进制位的运算）

| 运算符 | 运算       | 范例                                       |
| :----: | ---------- | ------------------------------------------ |
|   <<   | 左移       | 3<<2----->3 * 2 * 2= 12                    |
|   >>   | 右移       | 6>>2----->6 / (2 * 2) = 1 *注：小数点去掉* |
|  >>>   | 无符号右移 |                                            |
|   &    | 与运算     |                                            |
|   \|   | 或运算     |                                            |
|   ^    | 异或运算   |                                            |
|   ~    | 反码       |                                            |

```java
示例解析：
6 & 3 = 2

 0000-0000 0000-0000 0000-0000 0000-0110
&0000-0000 0000-0000 0000-0000 0000-0011
----------------------------------------
 0000-0000 0000-0000 0000-0000 0000-0010----->2
```

**注意：**

一个数异或同一个数两次，结果还是这个数。

**应用：**

数据的加密，比如，一个图片的数据异或一次，数据量不变，但是数据变了，再异或一次就可以得到原始数据。



左移：低位用0来补。

右移：对于高位出现的空位，原来高位是什么，就补什么。即最高位为1，就用1来补；最高位为0，就用0来补。

无符号右移：数据进行右移时，高位出现的空位，无论高位原来是什么，空位都用0来补。

**应用：**



**运算符练习：**

```java
class OperateTest
{
    public static void main(String[] args)
    {
        //有效的计算 2 * 8
        System.out.println(2 * 8);  //二进制乘法
        System.out.println(2<<3);  // 二进制移位更加有效
        
        //对两个整数变量的值进行互换
        /*
        实际开发使用的方式，使用第三方变量，阅读性强
        int a = 3, b = 5, c;
        c = a;
        a = b;
        b = c;
        System.out.print("a="+a+",b="+b);
        */
        
        //对两个整数变量的值进行互换（不需要第三方变量）
        //这种方式不建议使用，如果两个整数的数值过大，会超出int范围，会强制转换，数据会变化，出现精度问题
        /*
        int a = 3, b = 5;
        a = a + b;  // a = 3 + 5 = 8
        b = a - b;  // b = 8 - 5 = 3
        a = a - b;  // a = 8 - 3 = 5
        System.out.print("a="+a+",b="+b);
        */
        
        //对两个整数变量的值进行互换（不需要第三方变量）——位运算方法
        //运用异或运算的规律，面试的时候采用这种
        a = a ^ b;  // a = 3 ^ 5
        b = a ^ b;  // b = 3 ^ 5 ^ 5 = 3
        a = a ^ b;  // a = 3 ^ 5 ^ 3 = 5
    }
}
```

```java
// 二进制乘法演示
注：此处省略0000-0000 0000-0000 0000-0000 0000-0010只写了最后部分0010
2 * 8 = 16;
   0010
  *1000
-------
   0000
  0000
 0000
0010
-------
0010000----->16
```





### 三元运算符

- 格式
  - (条件表达式)? 表达式1: 表达式2;
  - 如果条件为true，运算后的结果是表达式1；
  - 如果条件为false，运算后的结果是表达式2；
- 示例
  - 获取两个数中大的数

```java
class OperateDemo7
{
    public static void main(String[] args)
    {
        /*
        int x =3, y;
        y = (x>1)?100:200;  //只要是运算符，必须有结果
        */
        
        //去两个数中大的那个
        int x =3, y = 4, z;
        z = (x>y)?x:y;
        System.out.println("z="+z);
        
        //获取三个整数中较大的整数
        int x = 3, y = 4, z = 5, temp;
        temp = (x>y)?x:y;
        max = (temp>z)?temp:z;
        System.out.println("max="+max);
    }
}
```



## 2.6 语句







## 流程控制

### 2.6.1 判断结构

```java
class IfDemo
{
    public static void main(String[] args)
    {
        /*
        if 语句的三种格式：
        1.
        if (条件表达式)  // if看的是条件表达式是否为true
        {
        	执行语句;
        }
        
        2.
        if (条件表达式)
        {
        	执行语句;
        }
        else
        {
        	执行语句;
        }
        
        3.
        if (条件表达式1)
        {
        	执行语句;
        }
        elif (条件表达式2)
        {
        	执行语句;
        }
        ...
        else
        {
        	执行语句;
        }
        
        
        */
    }
}
```

if语句必须明确自己的作用范围，当if语句控制单条语句时，可以简写去掉大括号，if控制离它最近的语句。

```java
int x = 3
if (x > 1)
    System.out.println("yes");  // if语句只控制这条语句
System.out.println("over");


int x = 3
if (x > 1)
{
    if (x < 5)
    {
        System.out.println("yes");
        System.out.println("yes");
    }
}
System.out.println("over");

简化为（省略大括号）：
int x = 3
    if (x > 1)
        if (x < 5)
        {
            System.out.println("yes");
        	System.out.println("yes");
        }
     System.out.println("over");
```

```java
int a = 3, b;
/*
if (a > 1)
	b = 100;
else
	b = 200;
*/
b = a>1?100:200;  //三元运算符就是if else语句的简写格式
b = a>1?sop(100):sop(200);  //简化格式必须有结果
System.out.println("b="+b);
```



### 2.6.2 选择结构



### 2.6.3 循环结构





## 数组







## 面向对象





## Java基础类库



## 泛型



## 异常





## IO流



File类的使用



IO流概述



节点流（或文件流）



缓冲流





转换流的使用

​	编码集



其他流的使用





对象流的使用

ObjectInputStream：存储中的文件、通过网络接手过来--->内存中的对象（序列化过程）

ObjectOutputStream：内存中的对象--->存储中的文件、通过网络传输出去（反序列化过程）







RandomAccessFile的使用









File、Path、Files的使用









## 反射

### 概述

加载完类之后，在堆内存中的方法区就产生一个 Class 类型的对象（一个类只有一个 Class 对象），这个对象就包含了完整的类的结构信息。我们可以通过这个对象看到类的结构。



```
正常方式：引入需要的“包类”名称--->通过new实例化-->取得实例化对象

反射方式：实例化对象--->getClass()方法--->得到完整的“包类”名称
```



反射相关的主要 API

java.lang.Class：代表一个类

java.lang.reflect.Method：代表类的方法

java.lang.reflect.Field：代表类的成员变量

java.lang.reflect.Construtor：代表类的构造器



例子：

```java

```







### 理解Class类并获取Class实例



获取Class实例的4种方式

```java

```



哪些类型可以有Class对象？

class：外部类

interface：

[]：

enum：

annotation：

primitive type：

void：



### 类的加载和ClassLoader的理解（学习 JVM）

类的加载：**将类的class文件读入内存，并为之创建一个java.lang.Class对象。此过程由类加载器完成。**



类的链接：将类的二进制数据合并到 JRE 中。



类的初始化：JVM 负责对类进行初始化。



```
源代码--->Java编译器--->.class文件--->类加载器--->字节码校验器--->解释器--->操作系统
```











### 创建运行时类的对象



```java
package com.imooc.test;

import org.junit.Test;

// 通过反射创建运行时类的对象
public class NewInstanceTest {
    @Test
    public void test1() throws IllegalAccessException, InstantiationException {
        /*
        newInstance()：调用此方法，创建对于的运行时类对象。内部调用了运行时类的空参构造器
        要想此方法正常的创建运行时类的对象，要求：
        1. 运行时类必须要有空参的构造器
        2. 空参的构造器的访问权限得够。通常设置为public

        在javabean中要求提供一个空参构造器。原因：
        1. 便于通过反射，创建运行时类的对象
        2. 便于子类继承运行时类时，默认调用super()时，保证父类有此构造器
         */
        Class clazz = Person.class;
        Person person = (Person) clazz.newInstance();
        System.out.println(person);
    }
}
```













### 获取运行时类的完整结构



```java
// 

```











### 调用运行时类的指定结构







### 反射的应用：动态代理（重点内容）

```java
// 体会反射的动态性
@Test
public void test2() {

    for (int i = 0; i < 100; i++) {
        int num = new Random().nextInt(3); // 0, 1, 2
        String classPath = "";
        switch(num) {
            case 0:
                classPath = "java.util.Date";
                break;
            case 1:
                classPath = "java.lang.Object";
                break;
            case 2:
                classPath = "com.imooc.test.Person";
                break;
        }

        try {
            Object obj = getInstance(classPath);
            System.out.println(obj);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

public Object getInstance(String classPath) throws Exception {
    Class clazz = Class.forName(classPath);
    return clazz.newInstance();
}
}
```





JDK 动态代理





CGLib 动态代理