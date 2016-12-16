---
layout: page
title: About
description: my blogs
keywords: xings
comments: true
menu: 关于
permalink: /about/
---

Technology

## 坚信

* 知行合一

## 联系

* 邮箱：xings_miracle@163.com
* 博客：[{{ site.title }}]({{ site.url }})

## Skill Keywords

#### Software Engineer Keywords
<div class="btn-inline">
    {% for keyword in site.skill_software_keywords %}
    <button class="btn btn-outline" type="button">{{ keyword }}</button>
    {% endfor %}
</div>

#### Tools Developer Keywords
<div class="btn-inline">
    {% for keyword in site.skill_tools_keywords %}
    <button class="btn btn-outline" type="button">{{ keyword }}</button>
    {% endfor %}
</div>
