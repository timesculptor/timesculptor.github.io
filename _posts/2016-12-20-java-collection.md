---
layout: post
title: java的Multimap 
categories: Java
description: java的Multimap。
keywords: Map
---

在实际应用中往往需要一个key对应多个value的情况，第三方的api实现。


## guava Multimap


```java

ListMultimap<String,String> multiMap = ArrayListMultimap.create();
        multiMap.put("a","b");
        multiMap.put("a", "c");
        List<String> value = multiMap.get("a");
                
```


## apache MultiValueMap

  
```java

MultiValueMap  multiValueMap = new MultiValueMap();
        multiValueMap.put("a","b");
        multiValueMap.put("a", "c");
        Collection a =multiValueMap.getCollection("a");
        
```

## Spring MultiValueMap


spring同样提供了MultiValueMap的一些基础操作，位于`package org.springframework.util`

```java

MultiValueMap<String,String> mutiMap = new LinkedMultiValueMap();
        mutiMap.add("a","b");
        mutiMap.add("a", "c");
        List<String> value = mutiMap.get("a");
        
```

MultiMap实现是通过封装Map<K, List<V>> targetMap






