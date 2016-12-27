---
layout: post
title: Map的遍历
categories: Java
description: Map。
keywords: Map
---

4种遍历map的方法。


## 通过map.keySet遍历

```java
Map<String,String> map = new HashMap<String,String>();
        map.put("key1", "value1");
        map.put("key2", "value2");
        map.put("key3", "value3");

        for(String key : map.keySet()) {
            String value = map.get(key);
            System.out.println(value);
        }
```

## 通过map.entrySet遍历

```java
Map<String, String> map = new HashMap<String, String>();
        map.put("key1", "value1");
        map.put("key2", "value2");
        map.put("key3", "value3");

        for (Map.Entry<String, String> entry : map.entrySet()) {
            String key = entry.getKey();
            String value = entry.getValue();
            System.out.println(value);
        }
```

## 通过map.values()


```java
Map<String, String> map = new HashMap<String, String>();
        map.put("key1", "value1");
        map.put("key2", "value2");
        map.put("key3", "value3");

        for (String v : map.values()) {
            String value = v;
            System.out.println(value);
        }
```


## 通过Map.entrySet迭代器

只和第二种遍历Map.entrySet的方式不同

```java
Map<String, String> map = new HashMap<String, String>();
        map.put("key1", "value1");
        map.put("key2", "value2");
        map.put("key3", "value3");

        Iterator<Map.Entry<String, String>> it = map.entrySet().iterator();
        while (it.hasNext()) {
            String value = it.next().getValue();
            System.out.println(value);
        }
```



