---
layout: post
title: Java反射机制
categories: Java
description: 反射。
keywords: reflect
---
反射机制动态地获取类的一切信息，并可以用这些信息做一些想做的事情。

## 反射机制

所谓反射，是指在运行时状态中，获取类中的属性和方法，以及调用其中的方法的一种机制。这种机制的作用在于获取运行时才知道的类（Class）及其中的属性（Field）、方法（Method）以及调用其中的方法，也可以设置其中的属性值。

Class文件由类装载器装载后，在JVM中将形成一份描述Class结构的元信息对象，通过该元信息对象可以获知Class的结构信息：如构造函数、属性和方法等。Java允许用户借由这个Class相关的元信息对象间接调用Class对象的功能。

## Class类

java.lang.Class类十分特殊，用来表示java中类型本身。
Class类是Reflection的根源。针对任何想动态加载、运行的类，唯有先获得相应的Class 对象。一个类被加载后，JVM会创建一个对应该类的Class对象，类的整个结构信息会放到对应的Class对象中。

Java中获取class类的对象有如下几种方式：

* 通过`Class.forName()`(最常用)
* 通过`.class 语法`
* 运用`getClass()`

例如：

```java
String path = "com.test.bean.User";
Class clazz1 = Class.forName(path);
          
Class strClazz = String.class;
Class intClazz =int.class;
  
Class strClazz2 = path.getClass();
```

## 应用反射API

通过反射机制访问java对象的属性，方法，构造方法
sun提供了反射机制中的类

```java
import java.lang.reflect.Constructor;
import java.lang.reflect.Method;
import java.lang.reflect.Field;
```

#### 获取类的信息(类的名字、属性、方法、构造器等)

有如下javabean：

```java
package com.test.bean;
public class User {
     
    private int id;
    private int age;
    private String name;
     
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    //必须要有无参的构造方法，否则构造对象的时候要先拿到有参数的构造方法，然后赋予参数再构建对象。
    public User() { }
    public User(int id,int age, String name) {
        this.id = id;
        this.age = age;
        this.name = name;
    }
}
```

获取类的信息demo


```java
public class Demo02 {
    public static void main(String[] args) throws NoSuchFieldException, ClassNotFoundException, NoSuchMethodException {
        String path = "com.test.bean.User";
        Class clazz = Class.forName(path);
        //获取类的名字
        System.out.println(clazz.getName());//获得包名+类名：com.test.bean.User
        System.out.println(clazz.getSimpleName());  //获的类名：User

        //获取属性信息
        //Field[] fields = clazz.getFields(); //只能获得public的field
        Field[] fields = clazz.getDeclaredFields();//获得所有的field
        Field f = clazz.getDeclaredField("name");
        System.out.println(fields.length);
        for (Field temp : fields) {
            System.out.println("属性：" + temp);
        }
        //获取方法信息
        Method[] methods = clazz.getDeclaredMethods();
        Method m01 = clazz.getDeclaredMethod("getName", null);
        //如果方法有参，则必须传递参数类型对应的class对象(有重载时可区分)
        Method m02 = clazz.getDeclaredMethod("setName", String.class);
        for (Method m : methods) {
            System.out.println("方法：" + m);
        }

        //获得构造器信息
        Constructor[] constructors = clazz.getDeclaredConstructors();
        //Constructor c = clazz.getDeclaredConstructor(int.class,int.class,String.class);
        //System.out.println("获得构造器："+c);
        for (Constructor temp : constructors) {
            System.out.println("构造器：" + temp);
        }
    }
}

```

输出结果：

```java
com.test.bean.User
User
3
属性：private int cn.qs.reflect.User.id
属性：private int cn.qs.reflect.User.age
属性：private java.lang.String cn.qs.reflect.User.name
方法：public java.lang.String cn.qs.reflect.User.getName()
方法：public void cn.qs.reflect.User.setName(java.lang.String)
方法：public int cn.qs.reflect.User.getId()
方法：public void cn.qs.reflect.User.setId(int)
方法：public int cn.qs.reflect.User.getAge()
方法：public void cn.qs.reflect.User.setAge(int)
构造器：public cn.qs.reflect.User(int,int,java.lang.String)
构造器：public cn.qs.reflect.User()
```

#### 动态的操作：构造器、方法、属性

```java
public class Demo03 {
    public static void main(String[] args) {
		String path = "com.test.bean.User";
		Class<User> clazz = (Class<User>) Class.forName(path);
		//通过反射API调用构造方法，构造对象
		User u = clazz.newInstance();   //其实是调用了User的无参构造方法
		System.out.println(u);
		Constructor<User> c = clazz.getDeclaredConstructor(int.class,int.class,String.class);
		User u2 = c.newInstance(1001,18,"Tim");
		System.out.println(u2.getName());

		//通过反射API调用普通方法
		User u3 = clazz.newInstance();
		Method method = clazz.getDeclaredMethod("setUname", String.class);
		method.invoke(u3, "tom");   //u3.setUname("tom");
		System.out.println(u3.getUname());

		//通过反射API操作属性
		User u4 = clazz.newInstance();
		Field f = clazz.getDeclaredField("uname");
		f.setAccessible(true); //这个属性不需要做安全检查了，可以直接访问
		f.set(u4, "Amay");       //通过反射直接写属性
		System.out.println(u4.getUname());  //通过反射直接读属性的值
		System.out.println(f.get(u4));
	}
}
```

输出结果：

```java
com.test.bean.User@723279cf
Tim
tom
Amay
Amay
```

## 反射性能问题

**`setAccessible`**

启用和禁用访问安全检查的开关，值为 true 则指示反射的对象在使用时应该取消 Java 语言访问检查。值为 false 则指示反射的对象应该实施 Java 语言访问检查。
禁止安全检查，可以提高反射的运行速度。

```java
public class Demo03 {
    public static void main(String[] args) throws IllegalAccessException, InstantiationException, NoSuchMethodException, InvocationTargetException, NoSuchFieldException, ClassNotFoundException {
        String path = "cn.qs.reflect.User";
        Class<User> clazz = (Class<User>) Class.forName(path);
        //通过反射API调用构造方法，构造对象
        User u = clazz.newInstance();   //其实是调用了User的无参构造方法
        System.out.println(u);
        Constructor<User> c = clazz.getDeclaredConstructor(int.class, int.class, String.class);
        User u2 = c.newInstance(1001, 18, "Tim");
        System.out.println(u2.getName());

        //通过反射API调用普通方法
        User u3 = clazz.newInstance();
        Method method = clazz.getDeclaredMethod("setName", String.class);
        method.invoke(u3, "tom");   //u3.setUname("tom");
        System.out.println(u3.getName());

        //通过反射API操作属性
        User u4 = clazz.newInstance();
        Field f = clazz.getDeclaredField("name");
        f.setAccessible(true); //这个属性不需要做安全检查了，可以直接访问
        f.set(u4, "Amay");       //通过反射直接写属性
        System.out.println(u4.getName());  //通过反射直接读属性的值
        System.out.println(f.get(u4));
    }
}
```

执行结果：

```java
普通方法调用，执行10亿次，耗时：2218ms
反射动态方法调用，执行10亿次，耗时：63427ms
反射动态方法调用，跳过安全检查，执行10亿次，耗时：14335ms
```

显然，通过测试发现反射比普通方法调用效率大大降低，但是可以通过禁止访问安全检查提高效率。万事都有利弊，反射降低了运行效率，但是灵活性提高了开发效率。



