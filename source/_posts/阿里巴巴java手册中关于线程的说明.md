---
title: '线程池原理'
date: 2019-10-10 18:21:48
tags: [java]
---
## 阿里巴巴java手册中关于线程的说明

1.【强制】创建线程或线程池时请指定有意义的线程名称，方便出错时回溯
正例:
```java
public class TimerTaskThread extends Thread { public TimerTaskThread() {
                  super.setName("TimerTaskThread");
... }
}
```
2.【强制】线程资源必须通过线程池提供，不允许在应用中自行显式创建线程。 说明:使用线程池的好处是减少在创建和销毁线程上所消耗的时间以及系统资源的开销，解决 资源不足的问题。如果不使用线程池，有可能造成系统创建大量同类线程而导致消耗完内存或 者“过度切换”的问题。
3.【强制】线程池不允许使用 Executors 去创建，而是通过 ThreadPoolExecutor 的方式，这样
的处理方式让写的同学更加明确线程池的运行规则，规避资源耗尽的风险。 说明:Executors 返回的线程池对象的弊端如下:

- FixedThreadPool 和 SingleThreadPool:
允许的请求队列长度为 Integer.MAX_VALUE，可能会堆积大量的请求，从而导致 OOM。

- CachedThreadPool 和 ScheduledThreadPool:
允许的创建线程数量为 Integer.MAX_VALUE，可能会创建大量的线程，从而导致 OOM。

## 线程池原理

###  参数及工作原理

首先是创建线程的 api：

```java
ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue, RejectedExecutionHandler handler) 
```

这几个核心参数的作用：

- `corePoolSize` 为线程池的基本大小。
- `maximumPoolSize` 为线程池最大线程大小。
- `keepAliveTime` 和 `unit` 则是线程空闲后的存活时间。
- `workQueue` 用于存放任务的阻塞队列。
- `handler` 当队列和最大线程池都满了之后的拒绝策略。

问题：
```java
new ThreadPoolExecutor(10, 20,0L, TimeUnit.MILLISECONDS,new LinkedBlockingQueue<Runnable>())
```
maximumPoolSize=20会不会起作用？


提交任务
1. 当前线程数量小于 coreSize 时创建一个新的线程运行,即使有空闲线程。
2. 当前线程数量大于 coreSize并且小于maximumPoolSize，会写入到队列中，只有当队列写满时才会继续创建新的线程
3. 如果coreSize等于maximumPoolSize，那么创建的是一个固定大小的线程池
4. 默认初始化的时候线程池中是没有线程的，只有当有新的任务提交的时候才会创建新线程，但是可以重写prestartCoreThread或者prestartAllCoreThreads方法，使得构造线程池的时候预先启动线程
5. 新线程是通过ThreadFactory创建的
6. 如果线程池里线程数超过corePoolSize，那么空闲时间超过keepAliveTime的线程会被终止掉，默认只会终止maximumPoolSize-coreSize那部分线程，allowCoreThreadTimeOut(true)可以使超时策略对核心线程同样起作用
7. 当线程数超过maximumPoolSize的时候，会采用拒绝策略，jdk提供了几种通用的策略，默认策略，抛出RejectedExecutionException
```
    /**
     * The default rejected execution handler
     */
    private static final RejectedExecutionHandler defaultHandler =
        new AbortPolicy();
```
8. 每个任务执行前后的钩子函数beforeExecute和afterExecute

具体看代码

### 关闭线程池

- `shutdown()` 执行后停止接受新任务，会把队列的任务执行完毕。
- `shutdownNow()` 也是停止接受新任务，但会中断所有的任务，将线程池状态变为 stop。

### 配置线程数
```
int cupNum = Runtime.getRuntime().availableProcessors();
```
一般线程数配置核数的1-2倍

## SpringBoot 使用线程池

```java
@Configuration
public class TreadPoolConfig {
    /**
     * 消费队列线程
     * @return
     */
    @Bean(value = "consumerQueueThreadPool")
    public ExecutorService buildConsumerQueueThreadPool(){
        ThreadFactory namedThreadFactory = new ThreadFactoryBuilder()
                .setNameFormat("consumer-queue-thread-%d").build();
        ExecutorService pool = new ThreadPoolExecutor(5, 5, 0L, TimeUnit.MILLISECONDS,
                new ArrayBlockingQueue<Runnable>(5),namedThreadFactory,new ThreadPoolExecutor.AbortPolicy());
        return pool ;
    }
}
```

使用时：

```java
    @Resource(name = "consumerQueueThreadPool")
    private ExecutorService consumerQueueThreadPool;
    @Override
    public void execute() {
        //消费队列
        for (int i = 0; i < 5; i++) {
            consumerQueueThreadPool.execute(new ConsumerQueueThread());
        }
    }
```


