---
layout: post
title: 单例补充 内部类方式
categories: DesignPatterns
description: 单例。
keywords: singleton
---
使用内部类实现单例模式。

## 内部类单例模式

这种单例模式使用java类级内部类和多线程缺省同步锁。

## 类级内部类
是指由static修饰的内部类，没有static修饰的是对象级内部类。类级内部类相当于其外部类的成员，只有在第一次被使用时才会被装载。jvm对一个类的初始化会做限制，同一时间只会允许一个线程去初始化一个类，这样就从虚拟机层面避免多线程问题。

## 多线程缺省同步锁

是指在某些情况下，JVM隐含的执行了同步synchronized，如初始换静态数据时会进行同步控制。

## 单例模式-内部类


```java
public class LazySingleton {
    
    private LazySingleton() { }

    private static LazySingleton lazySingleton = null;

    public static LazySingleton getInstance() {
        if (lazySingleton == null) {
            lazySingleton = new LazySingleton();
        }
        return lazySingleton;
    }
}  
```
