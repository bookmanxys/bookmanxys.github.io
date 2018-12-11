---
layout: post
title:  "mybaties- - generator"
categories: mybaties
tags:  generator 自动化代码
author: watermelon
---
* content
{:toc}

## 前言
mybaties的自动糊代码工具：generator，使用时有些地方需要注意





### 1：mysql版本问题注意
```xml
springboot-start-parent 2.1.0 ：的mysql-connector-java版本是6.x ，  
	跟上面一样配置也行，但是会提示：
	Loading class `com.mysql.jdbc.Driver'. This is deprecated. The new driver class is `com.mysql.cj.jdbc.Driver'. 
	The driver is automatically registered via the SPI and manual loading of the driver class is generally unnecessary.
	
	要配置成： com.mysql.cj.jdbc.Driver，并指定时区serverTimezone:
	url=jdbc:mysql://localhost:3306/test?serverTimezone=UTC&?useUnicode=true&characterEncoding=utf8&useSSL=false
	参考：https://www.cnblogs.com/liaojie970/p/8916568.html
	参考：https://blog.csdn.net/l_bestcoder/article/details/77529285
	
	还有mybatis-generator-core 在1.3.7版本是，对Mysql6.x 的功能会异常，只显示insert的方法
```

### 2：逆向过程中的字段类型有偏差
```xml
   <!--逆向问题 ： JavaTypeResolverDefaultImpl 可以看到源码-->
   <!--TINYINT字段长度 = 4 时 默认会逆向编译成byte-->
   <columnOverride column="is_dirty" javaType="java.lang.Integer" jdbcType="TINYINT"></columnOverride>
   <columnOverride column="is_dirty" javaType="java.lang.Boolean" jdbcType="TINYINT"></columnOverride>
   
   <!--text等类型会生生成另一个：withBOLG实体类，需要配置-->
   <columnOverride column="image_url" javaType="java.lang.String" jdbcType="VARCHAR"></columnOverride>
           
```



引用：  
[]()  
