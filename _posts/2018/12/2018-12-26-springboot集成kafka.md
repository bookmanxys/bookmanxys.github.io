---
layout: post
title:  "springboot集成kafka"
categories: springboot
tags:  kafka  队列
author: watermelon
---
* content
{:toc}

![kafka](https://wx3.sinaimg.cn/mw1024/005xB1vLly1fykhhli5x3j30k00b9jsz.jpg)
## 前言
springboot集成kafka



### 1、依赖
```xml
<dependency>
			<groupId>org.springframework.kafka</groupId>
			<artifactId>spring-kafka</artifactId>
		</dependency>
		
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>fastjson</artifactId>
			<version>1.2.47</version>
		</dependency>
		
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
		</dependency>
```
### 2、生产者
```java
import cn.faceland.springboot.common.constant.KafkaConstants;
import com.alibaba.fastjson.JSONObject;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.kafka.core.KafkaTemplate;
import org.springframework.kafka.support.SendResult;
import org.springframework.stereotype.Component;
import org.springframework.util.concurrent.ListenableFuture;
import org.springframework.util.concurrent.ListenableFutureCallback;

@Component
public class KafkaSender {

    private static final Logger logger = LoggerFactory.getLogger(KafkaSender.class);
    @Autowired
    private KafkaTemplate kafkaTemplate;


    public void send(Object obj){
        logger.info("准备发送消息为：{}",JSONObject.toJSONString(obj));
        //发送消息
        ListenableFuture<SendResult<String,Object>> future=kafkaTemplate.send(KafkaConstants.TOPIC_ONE,obj);
        future.addCallback(new ListenableFutureCallback<SendResult<String, Object>>() {
            @Override
            public void onFailure(Throwable throwable) {
                //发送失败的处理
                logger.info(KafkaConstants.TOPIC_ONE+" - 生产者 发送消息失败："+throwable.getMessage());
            }

            @Override
            public void onSuccess(SendResult<String, Object> stringObjectSendResult) {
                //成功的处理
                logger.info(KafkaConstants.TOPIC_ONE+" - 生产者 发送消息成功："+stringObjectSendResult.toString());
            }
        });


    }
}
```
### 3、消费者
```java
import cn.faceland.springboot.common.constant.KafkaConstants;
import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.kafka.annotation.KafkaListener;
import org.springframework.kafka.support.KafkaHeaders;
import org.springframework.messaging.handler.annotation.Header;
import org.springframework.stereotype.Component;

import java.util.Optional;

@Component
public class KafkaConsumer {
    private static final Logger logger = LoggerFactory.getLogger(KafkaConsumer.class);

    @KafkaListener(topics = KafkaConstants.TOPIC_ONE,groupId = KafkaConstants.TOPIC_ONE)
    public void topic_one(ConsumerRecord<?, ?> record, @Header(KafkaHeaders.RECEIVED_TOPIC) String topic){

        Optional message=Optional.ofNullable(record.value());
        if (message.isPresent()){
            Object msg=message.get();
            logger.info("被"+KafkaConstants.TOPIC_ONE+"消费了： +++++++++++++++ Topic:" + topic+",Record:" + record+",Message:" + msg);
        }
    }

}
```
### 4、测试
* 1：先做消费者，打jar包，命令窗口使用java -jar xxx.jar
* 2：然后运行test生产者 ，生产消息

```java
import cn.faceland.springboot.mq.KafkaSender;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

/**
 * @author watermelon on 2018/12/26 18:11
 * @description
 */
@RunWith(SpringRunner.class)
@SpringBootTest(classes = FacelandApplication.class)
public class KafkaTest {
    @Autowired
    KafkaSender kafkaSender;

    @Test
    public void producer(){
        kafkaSender.send("我是英雄123111");
    }
}
```

拓展：
* [SpringBoot集成kafka](https://my.oschina.net/648885471/blog/2046172)  