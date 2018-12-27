---
layout: post
title:  "springboot定时任务- -Spring Task"
categories: springboot
tags:  cron 定时任务
author: watermelon
---
* content
{:toc}

![定时任务](https://wx1.sinaimg.cn/mw1024/005xB1vLly1fyj8w28rwlj30k00b93za.jpg)
## 前言
整理下springboot下使用的定时任务方案






### 1：Spring Task

* cron("0/5 * * * * *") 
一个cron表达式有至少6个（也可能7个）有空格分隔的时间元素 
从左往右依次代表 
1.Seconds Minutes Hours DayofMonth Month DayofWeek Year 
2.Seconds Minutes Hours DayofMonth Month DayofWeek

```xml
Seconds:    可出现”, - * /”四个字符，有效范围为0-59的整数   
Minutes:    可出现”, - * /”四个字符，有效范围为0-59的整数   
Hours:      可出现”, - * /”四个字符，有效范围为0-23的整数   
DayofMonth :可出现”, - * / ? L W C”八个字符，有效范围为0-31的整数   
Month:      可出现”, - * /”四个字符，有效范围为1-12的整数或JAN-DEc   
DayofWeek:  可出现”, - * / ? L C #”四个字符，有效范围为1-7的整数或SUN-SAT两个范围。1表示星期天，2表示星期一， 依次类推   
Year:       可出现”, - * /”四个字符，有效范围为1970-2099年
```

各符号含义：
```xml
(1)*：表示匹配该域的任意值。假如在Minutes域使用*, 即表示每分钟都会触发事件。
(2)?：只能用在DayofMonth和DayofWeek两个域。它也匹配域的任意值，但实际不会。因为DayofMonth和DayofWeek会相互影响。例如想在每月的20日触发调度，不管20日到底是星期几，则只能使用如下写法： 13 13 15 20 * ?, 其中最后一位只能用？，而不能使用*，如果使用*表示不管星期几都会触发，实际上并不是这样。
(3)-：表示范围。例如在Minutes域使用5-20，表示从5分到20分钟每分钟触发一次 
(4)/：表示起始时间开始触发，然后每隔固定时间触发一次。例如在Minutes域使用5/20,则意味着5分钟触发一次，而25，45等分别触发一次. 
(5),：表示列出枚举值。例如：在Minutes域使用5,20，则意味着在5和20分每分钟触发一次。 
(6)L：表示最后，只能出现在DayofWeek和DayofMonth域。如果在DayofWeek域使用5L,意味着在最后的一个星期四触发。 
(7)W:表示有效工作日(周一到周五),只能出现在DayofMonth域，系统将在离指定日期的最近的有效工作日触发事件。例如：在 DayofMonth使用5W，如果5日是星期六，则将在最近的工作日：星期五，即4日触发。如果5日是星期天，则在6日(周一)触发；如果5日在星期一到星期五中的一天，则就在5日触发。另外一点，W的最近寻找不会跨过月份 。
(8)LW:这两个字符可以连用，表示在某个月最后一个工作日，即最后一个星期五。 
(9)#:用于确定每个月第几个星期几，只能出现在DayofMonth域。例如在4#2，表示某月的第二个星期三。
```

自己写的例子：
```xml
1：每5秒执行一次 0/5 * * * * *
2：每10分钟执行一次：0 0/10 * * * *
3：每小时的第10分钟和第30分执行一次；0 10,30 * * * *
4：每小时执行一次: 0 0 0/1 * * *
5：每个月第一天和第15天执行一次:0 0 0 1,15 * *
6：2002年至2005年的每月的最后一个星期五上午10:15触发：0 15 10 ? * 6L 2002-2005
```
 * fixedRate = 5000 两个start起点的时间间隔，上一个任务开始后等几秒开始先一个任务（如果上一个任务还没结束，就等着）
 （适合异步场景，配合@Async能够达到理想效果）
 * fixedDelay = 5000  end节点和start节点的时间间隔；上一个任务结束后，再固定等5秒后执行
 （适合单线程场景）
 
### 2: 代码
```java
@Slf4j
@Component
public class PrintTask {

    @Scheduled(cron = "0/5 * * * * *")
    public void scheduled(){
      log.info("=====>>>>>使用cron  {}",System.currentTimeMillis());
    }
    @Scheduled(fixedRate = 5000)
    public void scheduled1() {
        log.info("=====>>>>>使用fixedRate{}", System.currentTimeMillis());
    }
    @Scheduled(fixedDelay = 5000)
    public void scheduled2() {
        log.info("=====>>>>>fixedDelay{}",System.currentTimeMillis());
    }
}
  
结果：  
  
2018-12-25 21:02:25.002  INFO 8244 --- [   scheduling-1] cn.task.PrintTask    : =====>>>>>使用cron  1545742945002
2018-12-25 21:02:26.090  INFO 8244 --- [   scheduling-1] cn.task.PrintTask    : =====>>>>>使用fixedRate 1545742946090
2018-12-25 21:02:26.090  INFO 8244 --- [   scheduling-1] cn.task.PrintTask    : =====>>>>>fixedDelay 1545742946090
```

拓展：
* [cron表达式详解](https://www.cnblogs.com/javahr/p/8318728.html)  
* [SpringBoot几种定时任务的实现方式](https://www.cnblogs.com/zy-l/p/9178704.html)  
* [一张图让你秒懂Spring @Scheduled定时任务的fixedRate,fixedDelay,cron执行差异](https://blog.csdn.net/applebomb/article/details/52400154)  



