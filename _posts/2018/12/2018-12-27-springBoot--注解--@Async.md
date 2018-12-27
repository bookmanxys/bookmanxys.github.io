---
layout: post
title:  "springboot- -注解- -@Async"
categories: springboot
tags:  async
author: watermelon
---
* content
{:toc}

![SpringbootAsync](https://wxt.sinaimg.cn/mw1024/005xB1vLly1fylfgr5ag8j30k00b9wgx.jpg?tags=%5B%5D)
## 前言
springboot中使用集成的异步





### 1：说明
作为异步方案的一种，spring4之后支持async注解的方式提供异步方案。

注意：被打@Async注解的函数不能由本类内其他函数调用，必须是外部使用者调用，如果内部函数调用会出现代理绕过的问题，从而无法执行异步，不会出错，会变成同步操作。看起来就是@Async失效的状态。

### 2：代码
```java
//开启使用
@SpringBootApplication
@EnableAsync //开启异步调用
public class FacelandApplication {

	public static void main(String[] args) {
		SpringApplication.run(FacelandApplication.class, args);
	}

}
```

```java
//代码使用
@Component
public class TestJob {

    @Async("taskExecutor")
    public void startJob(int i){
        String msg = "我是任务【" + i + "】，开始了！时间：" + DateFormatUtils.format(System.currentTimeMillis(),"yyyy年MM月dd日 HH:mm:ss");
        System.out.println(msg);
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

```
### 3：配合线程池
再使用注解@Async是需要指明那个线程池，例如： @Async("taskExecutor")，否则会使用系统默认Executor  
```java
@EnableAsync
    @Configuration
    class TaskPoolConfig {

        @Bean("taskExecutor")
        public Executor taskExecutor() {
            ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
            executor.setCorePoolSize(10);
            executor.setMaxPoolSize(20);
            executor.setQueueCapacity(200);
            executor.setKeepAliveSeconds(60);
            executor.setThreadNamePrefix("taskExecutor-");
            executor.setRejectedExecutionHandler(new ThreadPoolExecutor.CallerRunsPolicy());
            return executor;
        }
    }

}
/**
上面我们通过使用ThreadPoolTaskExecutor创建了一个线程池，同时设置了以下这些参数：  
  
核心线程数10：线程池创建时候初始化的线程数  
最大线程数20：线程池最大的线程数，只有在缓冲队列满了之后才会申请超过核心线程数的线程  
缓冲队列200：用来缓冲执行任务的队列  
允许线程的空闲时间60秒：当超过了核心线程出之外的线程在空闲时间到达之后会被销毁  
线程池名的前缀：设置好了之后可以方便我们定位处理任务所在的线程池  
线程池对拒绝任务的处理策略：这里采用了CallerRunsPolicy策略，当线程池没有处理能力的时候，该策略会直接在 execute 方法的调用线程中运行被拒绝的任务；如果执行程序已关闭，则会丢弃该任务  
*/
```

引用：  
* [Spring Boot使用@Async实现异步调用：自定义线程池](https://www.cnblogs.com/moxiaotao/p/9777553.html)  
