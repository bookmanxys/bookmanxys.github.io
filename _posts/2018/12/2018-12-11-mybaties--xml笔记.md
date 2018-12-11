---
layout: post
title:  "mybaties- - xml收录"
categories: mybaties
tags:  xml sql
author: watermelon
---
* content
{:toc}

## 前言
mybaties的xml有些写法需要收录





### 1：mybatis中大于等于小于等于的写法
```xml
第一种写法（1）：
   
原符号       <        <=      >       >=       &        '        "
替换符号    &lt;    &lt;=   &gt;    &gt;=   &amp;   &apos;  &quot;
例如：sql如下：
create_date_time &gt;= #{startTime} and  create_date_time &lt;= #{endTime}
   
第二种写法（2）：
  
大于等于
<![CDATA[ >= ]]>
小于等于
<![CDATA[ <= ]]>
例如：sql如下：
create_date_time <![CDATA[ >= ]]> #{startTime} and  create_date_time <![CDATA[ <= ]]> #{endTime}
```

### 2：如何使用like
```java
    <if test="text != null and text.trim() != ''">
        and `text` like concat('%',#{text},'%')
    </if>
```

### 3：# 和 $ 的区别

 mybaties的#{} 和 ${} 在预编译中的处理是不一样的。  
 mybaties的#{} 在预处理时，会把参数部分用一个占位符 ? 代替  
 而 ${} 则只是简单的字符串替换
 以上，#{} 的参数替换是发生在 DBMS 中，而 ${} 则发生在动态解析过程中
 优先使用 #{}。因为 ${} 会导致 sql 注入的问题
 
 如果考虑数据权限的，并且做好的做够的安全检查都是可以使用sql拼接的方式实现数据权限


引用：  
[mybatis中"#"和"$"的区别](https://www.cnblogs.com/kangyun/p/5881531.html)  
