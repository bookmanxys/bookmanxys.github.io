---
layout: post
title:  "springboot--配置文件加载位置与优先级"
categories: springboot
tags:  配置文件 优先级
author: watermelon
---
* content
{:toc}

## 前言
继Memcached之后，考虑尝试选用Redis做缓存架构并使用分布式锁，先安装入门。






### 1：配置文件的优先级
spring boot 启动会扫描以下位置的application.properties或者application.yml文件作为Spring boot的默认配置文件
```xml
–file:./config/
–file:./
–classpath:/config/
–classpath:/
```
![配置文件的优先级](  https://img1.ph.126.net/ShcLK6XGLRL3UnBxnlieIg==/1967791562284725451.jpg)

### 2：全部优先级
springboot对配置文件设置了共17个优先级，上述配置文件分别是第11到第15。全部优先级如下：
```xml
1：Devtools global settings properties on your home directory (~/.spring-boot-devtools.properties when devtools is active).
2：@TestPropertySource annotations on your tests.
3：@SpringBootTest#properties annotation attribute on your tests.
4：Command line arguments.
5：Properties from SPRING_APPLICATION_JSON (inline JSON embedded in an environment variable or system property)
6：ServletConfig init parameters.
7：ServletContext init parameters.
8：JNDI attributes from java:comp/env.
9：Java System properties (System.getProperties()).
10：OS environment variables.
11：A RandomValuePropertySource that only has properties in random.*.
12：Profile-specific application properties outside of your packaged jar (application-{profile}.properties and YAML variants)
13：Profile-specific application properties packaged inside your jar (application-{profile}.properties and YAML variants)
14：Application properties outside of your packaged jar (application.properties and YAML variants).
15：Application properties packaged inside your jar (application.properties and YAML variants).
16：@PropertySource annotations on your @Configuration classes.
17：Default properties (specified using SpringApplication.setDefaultProperties).

```

参考：
* [参考官网地址](https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#boot-features-external-config)  
* [SpringBoot - 配置文件加载位置与优先级](https://blog.csdn.net/j080624/article/details/80508606)  



