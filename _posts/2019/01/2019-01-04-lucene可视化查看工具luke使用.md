---
layout: post
title:  "lucene之luke的使用"
categories: 搜索引擎
tags:  lucene 可视化工具 
author: watermelon
---
* content
{:toc}

![luke使用](https://images.gitee.com/uploads/images/2019/0104/101420_41d8d4a0_1210188.jpeg)
## 前言
这是真正的使用篇~






### 1：Overview选项卡
![luke页面信息分析](https://images.gitee.com/uploads/images/2019/0104/102114_0faa1529_1210188.jpeg)  
图中，显示了统计结果，Field、Document、分词数量  
其中field区域中统计了所有的Field，每个Field所包含的分词个数，百分比和编码格式。  
分词区域中是索引的详细信息，统计了每个分词出现的词频，以及对应的Field名字  
 
### 2：Documents选项卡
document选项卡用于文档的增删查改，下方的表格就像数据表一样，为我们展示每一个文件的具体数据，我们可以根据文档编号来查找文件。  

##### 新增document
点击Add按钮新增Document 
![addLuke](https://images.gitee.com/uploads/images/2019/0104/103201_a00918bd_1210188.jpeg)   
![addokLuke](https://images.gitee.com/uploads/images/2019/0104/103652_bad3d50a_1210188.jpeg)  
 
##### 更新document属性 
 点击Recoonstruct&Edit来更改当前Document的值与属性。
 ![updateokLuke](https://images.gitee.com/uploads/images/2019/0104/104134_a7ca7583_1210188.jpeg) 

### 3：Search选项卡
在这个界面可以进行索引的搜索测试，构造lucene的搜索语句，选择匹配的field字段，并执行查询。也可以选择进行索引的分词器，设置默认字段和重复搜索次数，设置限制查询时间及匹配个数、是否可以模糊匹配、选取哪种相似度的匹配模式、是否选用XML Query模式，还可以查看查询花费的时间等
![search](https://images.gitee.com/uploads/images/2019/0104/104821_23f70f67_1210188.jpeg)  

### 4：Commits选项卡
Comments选项卡用于查看每个索引文件的大小及相关属性，用于分析索引文件是否需要优化及合并等。  
![Commits](https://images.gitee.com/uploads/images/2019/0104/105236_2d91ebf6_1210188.jpeg)  

### 5：Plugins选项卡
这个页面是luke提供的各种插件，其中，Analyser Tool提供了一些分词的类，如图为luke分词的一个示例。Hadoop插件支持Hadoop（ 0.20.1）的任何文件系统打开索引。
包含图形化视图工具、导出工具等等

### 6：总结
luke是配合使用lucene的神器。辅助查询field、document、及分词的信息及数据统计；
可以通过使用luke检查索引的正确性，分析索引文件并进行修改和优化，将索引文件转换为易于阅读的XML格式，并且更直观地看到我们的documents；  
对于分词方面，luke可以加载分词包进行分词，进行词频统计及词汇增长统计，以及术语流行度统计等；对于搜索引擎，我们在构造查询语句之前，可以先使用luke进行查询语句校验，分析查询效率，更好地进行查询优化，这些对于设计一个更优秀的搜索引擎是很有必要的。


参考：
* [简书·使用Luke Lucene进行索引](https://www.jianshu.com/p/d9b24259daa8)  



