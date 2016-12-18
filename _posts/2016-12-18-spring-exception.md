---
layout: post
title: Spring mvc自定义全局异常处理
categories: Java
description: Spring mvc全局异常处理。
keywords: Spring
---

springmvc提供全局异常处理器（一个系统只有一个异常处理器）进行统一异常处理

##  全局异常
spring mvc可以通过实现HandlerExceptionResolver接口自定义异常。HandlerExceptionResolver中仅有一个方法

```java
	ModelAndView resolveException(
				HttpServletRequest request, HttpServletResponse 	response, Object handler, Exception ex);
```

## 使用方法

### 自定义全局异常方法
继承HandlerExceptionResolver并实现其方法

```java

public class GlobalExceptionResolver implements HandlerExceptionResolver {

    public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        if(!handler.getClass().isAnnotationPresent(ResponseBody.class)) {
            StringBuilder detail = new StringBuilder();
            ModelAndView exceptionView = new ModelAndView("error");
            if(ex != null) {
                StackTraceElement[] arr = ex.getStackTrace();
                int len = arr.length;
                for(int i = 0; i < len; ++i) {
                    StackTraceElement element = arr[i];
                    detail.append(element.toString().replace("\t", "").replace("\n", "").replace("\r", ""));
                }
                exceptionView.addObject("stackInfo", detail.toString());
            }
            exceptionView.addObject("errorMsg", ex.getMessage());
            return exceptionView;
        } else {
            ServiceResponseBase exceptionEntity = new ServiceResponseBase();
            exceptionEntity.setRetcode(-1);
            exceptionEntity.setRetdesc(ex.getMessage());
            String exceptionEntityJson = JSON.toJSONString(exceptionEntity);
            ModelAndView exceptionView = new ModelAndView("apierror");
            exceptionView.addObject("errorResponse", exceptionEntityJson);
            return exceptionView;
        }
    }
}


```

### 在mvc配置文件中配置全局异常处理器
  
```xml

<bean id="exceptionResolver"
          class="com.springapp.mvc.exception.GlobalExceptionResolver">
    </bean>

```

### 在mvc配置文件中配置全局异常处理器
error.vm

```xml
${errorMsg}
${stackInfo}
```

apierror.vm

```xml
${errorResponse}
```
##  示例





