---
layout: post
title:  "lucene可视化查看工具luke安装"
categories: 搜索引擎
tags:  lucene 可视化工具 
author: watermelon
---
* content
{:toc}

![luke可视化工具](https://images.gitee.com/uploads/images/2019/0104/093446_6cf72616_1210188.jpeg)
## 前言
整合lucene搜索引擎框架后，可以使用luke作为可视化工具进行查看索引和操作






### 1：luke可视化工具打开失败
【我的lucene版本是3.7.0】  
提示：No Valid directory at this location.Try another location  
```text
使用工具是：lukeall-3.5.0.jar
打开命令行窗口，执行命令: java -jar lukeall-3.5.0.jar
```
![3.5.0命令行窗口](https://images.gitee.com/uploads/images/2019/0104/095015_dcba111c_1210188.jpeg)
![3.5.0NoValid](https://images.gitee.com/uploads/images/2019/0104/095048_2d2340c6_1210188.jpeg)
 或者是read past EOF  
 ```text
 使用工具是：lukeall-0.9.9
 打开命令行窗口，执行命令: java -jar lukeall-0.9.9
 ```
![read past EOF](https://images.gitee.com/uploads/images/2019/0104/094703_bc529dd9_1210188.jpeg)

### 2:需要跟lucene版本一致
```text
下载网址：（根据lucene版本，下载对应版本的luke）
github：https://github.com/DmitryKey/luke/releases
```
![githubluke](https://images.gitee.com/uploads/images/2019/0104/095405_0f18e4be_1210188.jpeg)

解压后，双击（.bat和.sh都行）运行：
![解压luke](https://images.gitee.com/uploads/images/2019/0104/095518_5c82191f_1210188.jpeg)
![双击luke](https://images.gitee.com/uploads/images/2019/0104/100022_319c1536_1210188.jpeg)
![okluke](https://images.gitee.com/uploads/images/2019/0104/100055_ed84e519_1210188.jpeg)

安装成功

### 3:可能会出异常：
 ```text
 在luke打开索引时报错：
java.lang.ArrayIndexOutOfBoundsException: 0
```
 可能未commit，导致索引异常，检查下代码中是否有indexWriter.commit()
 当然也有可能是没有索引导致的，添加上去就行...
 ![nomalluke](https://images.gitee.com/uploads/images/2019/0104/100642_3a5f07ce_1210188.jpeg)
 
 ### 4：如何使用
 * [luke使用篇](https://blog.csdn.net/yelllowcong/article/details/78698595)  

参考：
* [Lucene之索引查看工具Luke-yellowcong](https://blog.csdn.net/yelllowcong/article/details/78698595)  
* [Lucene问题之：java.lang.ArrayIndexOutOfBoundsException](https://blog.csdn.net/qq_14838603/article/details/84984165)  



