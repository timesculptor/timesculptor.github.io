---
layout: post
title: Java反射机制
categories: Java
description: 反射。
keywords: reflect
---
给定类的名字，就可以通过反射机制来获得类的所有信息。
@(反射)
## 反射机制

所谓反射，是指在运行时状态中，获取类中的属性和方法，以及调用其中的方法的一种机制。这种机制的作用在于获取运行时才知道的类（Class）及其中的属性（Field）、方法（Method）以及调用其中的方法，也可以设置其中的属性值。

Class文件由类装载器装载后，在JVM中将形成一份描述Class结构的元信息对象，通过该元信息对象可以获知Class的结构信息：如构造函数、属性和方法等。Java允许用户借由这个Class相关的元信息对象间接调用Class对象的功能。

## Class类

java.lang.Class类十分特殊，用来表示java中类型本身。
Class类是Reflection的根源。针对任何想动态加载、运行的类，唯有先获得相应的Class 对象。一个类被加载后，JVM会创建一个对应该类的Class对象，类的整个结构信息会放到对应的Class对象中。

Java中获取class类的对象有如下几种方式：
- 通过`Class.forName()`(最常用)
- 通过`.class 语法`
- 运用`getClass()`

例如下面三种方式：

```java

String path = "com.test.bean.User";
Class clazz1 = Class.forName(path);
          
Class strClazz = String.class;
Class intClazz =int.class;
  
Class strClazz2 = path.getClass();
  
```
## 反射具体功能实现
通过反射机制访问java对象的属性，方法，构造方法
sun提供了反射机制中的类

```java
import java.lang.reflect.Constructor;
import java.lang.reflect.Method;
import java.lang.reflect.Field;
```

**应用反射的API，获取类的信息(类的名字、属性、方法、构造器等)**
```java
public class Demo02 {
	public static void main(String[] args) {
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
		for(Field temp:fields){
			System.out.println("属性："+temp);
		}
		//获取方法信息
		Method[] methods = clazz.getDeclaredMethods();
		Method m01 = clazz.getDeclaredMethod("getName", null);
		//如果方法有参，则必须传递参数类型对应的class对象(有重载时可区分)
		Method m02 = clazz.getDeclaredMethod("setUname", String.class); 
		for(Method m:methods){
			System.out.println("方法："+m);
		}
             
		//获得构造器信息
		Constructor[] constructors = clazz.getDeclaredConstructors();
		Constructor c = clazz.getDeclaredConstructor(int.class,int.class,String.class);
		System.out.println("获得构造器："+c);
		for(Constructor temp:constructors){
			System.out.println("构造器："+temp);
		}
	}
}         
```
**通过反射API动态的操作：构造器、方法、属性**
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
####反射性能问题
**`setAccessible`**
- 启用和禁用访问安全检查的开关,值为 true 则指示反射的对象在使用时应该取消 Java 语言访问检查。值为 false 则指示反射的对象应该实施 Java 语言访问检查。并不是为true就能访问为false就不能访问。
-  禁止安全检查，可以提高反射的运行速度。
```java
public static void test01(){
        User u = new User();
        long startTime = System.currentTimeMillis();
        for (int i = 0; i < 1000000000L; i++) {
            u.getName();
        }
        long endTime = System.currentTimeMillis();
        System.out.println("普通方法调用，执行10亿次，耗时："+(endTime-startTime)+"ms"); 
    }
     
    public static void test02() throws Exception{
        User u = new User();
        Class clazz = u.getClass();
        Method m = clazz.getDeclaredMethod("getName", null);
        //m.setAccessible(true);
		long startTime = System.currentTimeMillis(); 
        for (int i = 0; i < 1000000000L; i++) {
            m.invoke(u, null);
        }
        long endTime = System.currentTimeMillis();
        System.out.println("反射动态方法调用，执行10亿次，耗时："+(endTime-startTime)+"ms");
    }
     
    public static void test03() throws Exception{
        User u = new User();
        Class clazz = u.getClass();
        Method m = clazz.getDeclaredMethod("getName", null);
        m.setAccessible(true);  //不需要执行访问安全检查
        long startTime = System.currentTimeMillis();
        for (int i = 0; i < 1000000000L; i++) {
            m.invoke(u, null);
        }
         
        long endTime = System.currentTimeMillis();
        System.out.println("反射动态方法调用，跳过安全检查，执行10亿次，耗时："+(endTime-startTime)+"ms");
    }
     public static void main(String[] args) throws Exception {
        test01();
        test02();
        test03();
    }
```
执行结果：
```
普通方法调用，执行10亿次，耗时：2218ms
反射动态方法调用，执行10亿次，耗时：63427ms
反射动态方法调用，跳过安全检查，执行10亿次，耗时：14335ms
```
<pre>
显然，通过测试发现反射比普通方法调用效率大大降低，但是可以通过禁止访问安全检查提高效率。万事都有利弊，反射降低了运行效率，但是灵活性提高了开发效率。
</pre>


