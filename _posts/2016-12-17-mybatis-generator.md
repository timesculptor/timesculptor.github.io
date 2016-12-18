---
layout: post
title: Mybatis逆向工程
categories: Java
description: mybatis逆向工程-mybatis-generator。
keywords: Mybatis
---

mybatis官方提供逆向工程 可以针对单表自动生成mybatis执行所需要的代码

## 逆向工程
MyBatis Generator(MBG)，可以逆向生成持久层的基本代码，可以自由组合完成比较复杂的查询，maven依赖 MyBatis Generator Core。


[官方文档](http://www.mybatis.org/generator/configreference/xmlconfig.html)


## 使用方法
### 修改配置文件

配置逆向工程配置文件 genreatorConfig.xml，（[官方配置说明](http://www.mybatis.org/generator/configreference/xmlconfig.htl)），主要配置以下几处

  1. 数据库连接信息

  2. 指定生成文件存放的路径

  3. 指定数据库中表

  
```xml

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
  PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
  "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
	<context id="testTables" targetRuntime="MyBatis3">
		<commentGenerator>
			<!-- 是否去除自动生成的注释 true：是 ： false:否 -->
			<property name="suppressAllComments" value="true" />
		</commentGenerator>
		<!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
		<jdbcConnection driverClass="com.mysql.jdbc.Driver"
			connectionURL="jdbc:mysql://localhost:3306/testdb"
						userId="root"
			password="123456">
		</jdbcConnection>
		<!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为 true时把JDBC DECIMAL 和 
			NUMERIC 类型解析为java.math.BigDecimal -->
		<javaTypeResolver>
			<property name="forceBigDecimals" value="false" />
		</javaTypeResolver>

		<!-- targetProject:生成PO类的位置 -->
		<javaModelGenerator targetPackage="cn.qs.model"
			targetProject="src/main/java">
			<!-- enableSubPackages:是否让schema作为包的后缀 -->
			<property name="enableSubPackages" value="false" />
			<!-- 从数据库返回的值被清理前后的空格 -->
			<property name="trimStrings" value="true" />
		</javaModelGenerator>
        <!-- targetProject:mapper映射文件生成的位置 -->
		<sqlMapGenerator targetPackage="cn.qs.mapper"
			targetProject="src/main/java">
			<!-- enableSubPackages:是否让schema作为包的后缀 -->
			<property name="enableSubPackages" value="false" />
		</sqlMapGenerator>
		<!-- targetPackage：mapper接口生成的位置 -->
		<javaClientGenerator type="XMLMAPPER"
			targetPackage="cn.qs.mapper"
			targetProject="src/main/java">
			<!-- enableSubPackages:是否让schema作为包的后缀 -->
			<property name="enableSubPackages" value="false" />
		</javaClientGenerator>
		<!-- 指定数据库表 -->
		<table tableName="qqvip_card_data"></table>
		<table tableName="qqvip_card_config"></table>

	</context>
</generatorConfiguration>


```

### 运行java生成程序

生成 mapper.java、mapper.xml、Example.java、po等文件。

```java
public class GeneratorSqlmap {

	public void generator() throws Exception{

		List<String> warnings = new ArrayList<String>();
		boolean overwrite = true;
		//指定 逆向工程配置文件
		String genConfig = "conf/core/generatorConfig.xml";
		ClassLoader classLoader = getClass().getClassLoader();
		File configFile = new File(classLoader.getResource(genConfig).getFile());
		ConfigurationParser cp = new ConfigurationParser(warnings);
		Configuration config = cp.parseConfiguration(configFile);
		DefaultShellCallback callback = new DefaultShellCallback(overwrite);
		MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config,
				callback, warnings);
		myBatisGenerator.generate(null);

	} 
	public static void main(String[] args) throws Exception {
		try {
			GeneratorSqlmap generatorSqlmap = new GeneratorSqlmap();
			generatorSqlmap.generator();
		} catch (Exception e) {
			e.printStackTrace();
		}
		
	}

}


```
使用生成的文件

![](/images/posts/java/mybatis-example.jpeg)

## 示例
genreatorConfig.xml文件配置

java代码生成

具体示例参见 github: [mybatis-generator](https://github.com/timesculptor/mybatis-generator)

