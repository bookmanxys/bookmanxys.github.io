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
有时候配置了时区还不管用
*  异常打印：“The server time zone value 'ÖÐ¹ú±ê×¼Ê±¼ä' is unrecognized or represents more than one time zone. Yo”
```sql
//查一下当前库的时区信息
show variables like '%time_zone%'

//update一下
set global time_zone='+8:00'

//这个不能update，set global system_time_zone='CST'：因为它是只读模式readonly
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

### 3: 会出现逆向生成时将所有不同库但相同表名的生成在了一起
```xml
13:23:04.460 [main] DEBUG org.mybatis.generator.internal.db.DatabaseIntrospector - Found column "id", data type 4, in table "数据库2..tb_role"
13:23:04.460 [main] DEBUG org.mybatis.generator.internal.db.DatabaseIntrospector - Found column "roleName", data type 12, in table "数据库2..tb_role"
13:23:04.460 [main] DEBUG org.mybatis.generator.internal.db.DatabaseIntrospector - Found column "roleCode", data type 12, in table "数据库2..tb_role"
13:23:04.461 [main] DEBUG org.mybatis.generator.internal.db.DatabaseIntrospector - Found column "seq", data type 4, in table "数据库2..tb_role"
13:23:04.461 [main] DEBUG org.mybatis.generator.internal.db.DatabaseIntrospector - Found column "disable", data type 4, in table "数据库2..tb_role"
13:23:04.461 [main] DEBUG org.mybatis.generator.internal.db.DatabaseIntrospector - Found column "id", data type 4, in table "数据库3..tb_role"
13:23:04.461 [main] DEBUG org.mybatis.generator.internal.db.DatabaseIntrospector - Found column "roleName", data type 12, in table "数据库3..tb_role"
13:23:04.461 [main] DEBUG org.mybatis.generator.internal.db.DatabaseIntrospector - Found column "roleCode", data type 12, in table "数据库3..tb_role"
13:23:04.461 [main] DEBUG org.mybatis.generator.internal.db.DatabaseIntrospector - Found column "seq", data type 4, in table "数据库3..tb_role"
13:23:04.461 [main] DEBUG org.mybatis.generator.internal.db.DatabaseIntrospector - Found column "id", data type 4, in table "数据库1..tb_role"
13:23:04.461 [main] DEBUG org.mybatis.generator.internal.db.DatabaseIntrospector - Found column "role_name", data type 12, in table "数据库1..tb_role"
13:23:04.461 [main] DEBUG org.mybatis.generator.internal.db.DatabaseIntrospector - Found column "role_code", data type 12, in table "数据库1..tb_role"
13:23:04.461 [main] DEBUG org.mybatis.generator.internal.db.DatabaseIntrospector - Found column "seq", data type 4, in table "数据库1..tb_role"
13:23:04.461 [main] DEBUG org.mybatis.generator.internal.db.DatabaseIntrospector - Found column "disable", data type -7, in table "数据库1..tb_role"
13:23:04.461 [main] DEBUG org.mybatis.generator.internal.db.DatabaseIntrospector - Found column "foid", data type 12, in table "数据库1..tb_role"
13:23:04.463 [main] DEBUG org.mybatis.generator.internal.db.DatabaseIntrospector - Found column "loid", data type 12, in table "数据库1..tb_role"
13:23:04.463 [main] DEBUG org.mybatis.generator.internal.db.DatabaseIntrospector - Found column "created", data type 93, in table "数据库1..tb_role"
13:23:04.463 [main] DEBUG org.mybatis.generator.internal.db.DatabaseIntrospector - Found column "modified", data type 93, in table "数据库1..tb_role"
13:23:04.464 [main] DEBUG org.mybatis.generator.internal.db.DatabaseIntrospector - Found override for column "disable" in table "数据库1..tb_role"
13:23:04.464 [main] DEBUG org.mybatis.generator.internal.db.DatabaseIntrospector - Found override for column "disable" in table "数据库2..tb_role"
```
看得出，所有不同库但相同表名的生成了，再xml文件里重复出现了
```xml
出现三次：
<resultMap id="BaseResultMap" ...>
```

处理方法：

```xml
 <table catalog="指定库名" tableName="tb_role" domainObjectName="Role" ...>
    <!-- 默认为false，如果设置为true，在生成的SQL中，table名字不会加上限定词:catalog或schema；-->
    <property name="ignoreQualifiersAtRuntime" value="true"/>
	...
 </table>
```

### 4：generator.xml内节点必须要按顺序写
```xml
Exception in thread "main" org.mybatis.generator.exception.XMLParserException: 
XML Parser Error on line 83: 元素类型为 "context" 的内容必须匹配 "(property*,plugin*,commentGenerator?,(connectionFactory|jdbcConnection),javaTypeResolver?,javaModelGenerator,sqlMapGenerator?,javaClientGenerator?,table+)"。
  
  必须按顺序写，不然会报错
```

### 5：可以添加一些插件
* 实体类加上序列化：
```xml
<plugin type="org.mybatis.generator.plugins.SerializablePlugin"></plugin>
```
* 实体类加上序列化2：
```xml
<plugin type="cn.faceland.springboot.common.generator.plugins.EntitySerializablePlugin" />
```
* toString：
 ```xml
 <plugin type="org.mybatis.generator.plugins.ToStringPlugin" />
```
* 分页：
 ```xml
 <plugin type="cn.faceland.springboot.common.generator.plugins.MySQLPaginationPlugin"></plugin>
```
* 自增长主键：
 ```xml
<plugin type="cn.faceland.springboot.common.generator.plugins.MySQLGeneratorPrimaryKeyPlugin"></plugin>
```
* 库表注释： 
 ```xml
    <commentGenerator type="cn.faceland.springboot.common.generator.plugins.CommentGenerator">
        <property name="suppressDate" value="true"/>
        <property name="suppressAllComments" value="true" />
    </commentGenerator>
```

引用：  
[【项目管理】Mybatis-Generator之最完美配置详解](https://blog.csdn.net/zsq520520/article/details/50952830)  
[简介如何使用MyBatis generator生成的Example文件](https://blog.csdn.net/m0_37795198/article/details/78848045)  
[mybatis-generator同名表的处理](https://blog.csdn.net/qq_41872909/article/details/82855750)  
[mysql的时区错误问题](https://blog.csdn.net/weixin_39033443/article/details/81711306)  
