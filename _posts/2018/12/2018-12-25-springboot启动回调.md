---
layout: post
title:  "springboot启动回调"
categories: springboot
tags:  回调
author: watermelon
---
* content
{:toc}

![启动回调](https://wx3.sinaimg.cn/mw1024/005xB1vLly1fyja583uxrj30k00b9mzm.jpg)
## 前言
springboot打成可执行jar包，提供了容器环境。怎么样回调一次性任务（非定时任务）
CommandLineRunner、ApplicationRunner 接口是在容器启动成功后的最后一步回调（类似开机自启动）。






### 1：CommandLineRunner方案
```java
@Order(1) //控制顺序
@Component
public class PrintSuccessRunner implements CommandLineRunner {
    @Override
    public void run(String... args) throws Exception {
        System.out.println("PrintSuccessRunner:i am call back~");
    }
}
```

```java
@Order(2)
@Component
public class PrintStartRunner implements CommandLineRunner {
    @Override
    public void run(String... args) throws Exception {
        System.out.println("PrintStartRunner:i am call back~");
    }
}
```

### 2: ApplicationRunner方案
```java
@Component
public class MyApplicationRunner implements ApplicationRunner{

    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("===MyApplicationRunner==="+ Arrays.asList(args.getSourceArgs()));
        System.out.println("===getOptionNames========"+args.getOptionNames());
        System.out.println("===getOptionValues======="+args.getOptionValues("foo"));
        System.out.println("==getOptionValues========"+args.getOptionValues("developer.name"));
    }
}
```
还没搞明白，打了包之后如何传参 -_-

拓展：
* [CommandLineRunner或者ApplicationRunner接口](https://www.jianshu.com/p/5d4ffe267596)  



