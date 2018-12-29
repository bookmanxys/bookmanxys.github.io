---
layout: post
title:  "maven- - 打包跳过test"
categories: 团队开发
tags:  jar maven test
author: watermelon
---
* content
{:toc}

## 前言
使用了maven构建springboot项目，在打包jar（mvn install）时发现会测试连接数据源是否能够连接。但是本地打包时本地ip并不在生产环境的ip白名单，所以想办法跳过test





### 1：方法一：修改pom.xml文件
```xml
<project>  
  [...]  
  <build>  
    <plugins>  
      <plugin>  
        <groupId>org.apache.maven.plugins</groupId>  
        <artifactId>maven-surefire-plugin</artifactId>  
        <version>2.18.1</version>  
        <configuration>  
          <skipTests>true</skipTests>  
        </configuration>  
      </plugin>  
    </plugins>  
  </build>  
  [...]  
</project>
```

### 方法二：在Terminal执行命令
```xml
我用的intellij IDEA开发编译工具  
   
mvn install -DskipTests
```

### 方法三：在Terminal执行命令
```xml
mvn install -Dmaven.test.skip=true
```

我就使用了第一种，有效~

引用：  
* [maven打包时跳过测试](https://blog.csdn.net/so_geili/article/details/79986789)  
* [深入理解maven构建生命周期和各种plugin插件](https://blog.csdn.net/zhaojianting/article/details/80321488)  
