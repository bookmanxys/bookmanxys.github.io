---
layout: post
title:  "mybaties- - Example用法"
categories: mybaties
tags:  generator
author: watermelon
---
* content
{:toc}

## 前言
mybaties的generator自动生成代码后，实体类的example类可以省下大部分写xml的功夫，通过设置条件Criteria做复杂且自由的查询





### 1：例子
开启后生成的mapper中会包含ByExample的方法：
```xml
 enableCountByExample="true" enableUpdateByExample="true" enableDeleteByExample="true"
               enableSelectByExample="true" selectByExampleQueryId="true" >
```
比较详细的例子：
```java
List<User> list = new ArrayList<>();
//1.模糊搜索用户名：
String name = "测";
UserExample ex = new UserExample();
ex.createCriteria().andUserNameLike('%'+name+'%');
//list = UserMapper.selectByExample(ex);
  
//2.通过某个字段排序：
String orderByClause = "seq desc, id asc";
ex = new UserExample();
ex.setOrderByClause(orderByClause);
//list = UserMapper.selectByExample(ex);
  
//3.条件搜索，不确定条件的个数：
User User = new User();
User.setUserName("后");
ex = new UserExample();
UserExample.Criteria criteria = ex.createCriteria();
if(StringUtils.isNotEmpty(User.getUserName())){
    criteria.andUserNameLike('%' + User.getUserName() + '%');
}
  
if(StringUtils.isNotEmpty(User.getPassword())){
    criteria.andPasswordEqualTo(User.getPassword());
}
//list = UserMapper.selectByExample(ex);
  
//4.分页
ex = new UserExample();
Pagination page = new Pagination();
page.setBegin(0);
page.setLength(10);
ex.setPagination(page);
list = userMapper.selectByExample(ex);
  
for(User r : list){
    System.out.println(r.toString());
}
```



引用：  
* [简介如何使用MyBatis generator生成的Example文件](https://blog.csdn.net/m0_37795198/article/details/78848045)  
* [mapper接口中的方法解析](https://blog.csdn.net/biandous/article/details/65630783)  
