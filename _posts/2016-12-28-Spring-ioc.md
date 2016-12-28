---
layout: post
title: Spring Bean对IoC容器的感知
categories: Spring
description: ioc。
keywords: ioc
---
XXXAware在spring里表示XXX可感知。

## Aware接口

Spring有许多Aware结尾接口，实现这些Aware接口的bean在被初始化之后，可以取得一些对应的资源。在初始化bean时，会检测是是否实现了这些接口并调用接口中的setter方法。

| Aware接口                     | 功能                                |
|:--------------------------|:--------------------------------------|
|    BeanNameAware | 可以在bean中得到它在IoC容器中的bean实例名称 |
| BeanFactoryAware） | 得到bean所在的IoC容器                |
| ApplicationContentAware| 得到bean所在的应用上下文                |
| ApplicationEventPublisherAware   | 得到bean所在的应用上下文事件发布器|
| ServletConfigAware     | web ServletConfig            |
| ServletContextAware    | web ServletContext     |
| EnvironmentAware         | web 环境         |
| ResourceLoaderAware       | 资源加载器       |



## ApplicationContentAware

ApplicationContentAware可以在bean中得到bean所在的应用上下文，从而直接在bean中使用应用上下文的服务。
初始化bean时将applicationContent引用在bean中保存下来。

### 应用


```java
public class SpringContext implements ApplicationContextAware{

    private static ApplicationContext applicationContext;

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        SpringContext.applicationContext = applicationContext;

    }

    public static <T> T getBean(String name) {
        return (T) applicationContext.getBean(name);
    }
}
```

同时在xml中将该类配置成bean

```xml
<bean class="com.springapp.mvc.common.util.SpringContext"></bean>
```

之后既可以在其他地方通过`SpringContext.getBean("beanName")`得到容器中的bean

## EnvironmentAware

EnvironmentAware可以获得mvc下的web环境ServletContext、ServletConfig、系统环境变量、系统属性等都封装在其propertySource属性下。

```java
@Controller
@RequestMapping("/webtest")
public class HelloController implements EnvironmentAware{

	private Environment environment;

	@Override
	public void setEnvironment(Environment environment) {
		this.environment = environment;
		String path = environment.getProperty("log4jConfigLocation");
	}
}
```

断点结果：

![](/images/posts/java/spring-environment.jpeg)
