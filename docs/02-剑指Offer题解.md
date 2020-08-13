# 剑指Offer题解

> 题目1：设计一个类，我们只能生成该类的一个实例。

解题思路：

使用一个私有构造函数、一个私有静态变量以及一个公有静态函数来实现。

```java
public class Sinleton {
    // 
    private static Sinleton singleton;
    
    private Singleton() {}
    
    public static Sinleton getSingleton() {
        if (sinleton == null) {
            singleton = new Singleton();
        }
        return singleton;
    }
}
```

当多个线程同时判断是否为空的时候，会出现线程安全问题。





