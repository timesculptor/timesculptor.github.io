---
layout: post
title: java的Multimap 
categories: Java
description: java的Multimap。
keywords: Map
---

在实际应用中往往需要一个key对应多个value的情况，介绍几个第三方的api实现。


## guava Multimap

package com.google.common.collect;

```java

ListMultimap<String,String> multiMap = ArrayListMultimap.create();
        multiMap.put("a","b");
        multiMap.put("a", "c");
        List<String> value = multiMap.get("a");
                
```
对集合的扩充和增强，无论从简洁、易读和易维护等方面，gauva都是首选的。

## apache MultiValueMap
![](/images/posts/java/Apache-collection.jpeg)
  
```java

MultiValueMap  multiValueMap = new MultiValueMap();
        multiValueMap.put("a","b");
        multiValueMap.put("a", "c");
        Collection a =multiValueMap.getCollection("a");
        
```

## Spring MultiValueMap


最后是spring同样提供了MultiValueMap的一些基础操作，位于`package org.springframework.util`


```java

MultiValueMap<String,String> mutiMap = new LinkedMultiValueMap();
        mutiMap.add("a","b");
        mutiMap.add("a", "c");
        List<String> value = mutiMap.get("a");
        
```

spring的实现最简单，通过封装```Map<K, List<V>> targetMap```j结构
add 方法
```java

private final Map<K, List<V>> targetMap;

public LinkedMultiValueMap() {
		this.targetMap = new LinkedHashMap<K, List<V>>();
	}
	
public void add(K key, V value) {
		List<V> values = this.targetMap.get(key);
		if (values == null) {
			values = new LinkedList<V>();
			this.targetMap.put(key, values);
		}
		values.add(value);
	}
```





