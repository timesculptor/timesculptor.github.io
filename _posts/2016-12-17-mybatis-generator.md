---
layout: post
title: Mybatis逆向工程
categories: Java
description: mybatis逆向工程-mybatis-generator。
keywords: Mybatis
---

mybatis官方提供逆向工程 可以针对单表自动生成mybatis执行所需要的代码

### 逆向工程
MyBatis Generator(MBG)，可以逆向生成持久层的基本代码，可以自由组合完成比较复杂的查询。


[官方文档](http://www.mybatis.org/generator/configreference/xmlconfig.html)


### maven依赖
* MyBatis Generator Core
* MySQL Connector/J
* Mybatis

### 使用方法
* 配置逆向工程配置文件 genreatorConfig.xml，（[官方配置说明](http://www.mybatis.org/generator/configreference/xmlconfig.htl)），主要配置以下几处

  1. 数据库连接信息

  2. 指定生成文件存放的路径

  3. 指定数据库中表

* 运行java生成程序，生成 mapper.java、mapper.xml、Example.java、po等文件。
* 使用生成的文件

![](/images/posts/java/mybatis-example.jpeg)

### 示例
genreatorConfig.xml文件配置

java代码生成

具体示例参见 github: [mybatis-generator](https://github.com/timesculptor/mybatis-generator)

