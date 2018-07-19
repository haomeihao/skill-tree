#### java-concurrency Java并发包
```
概念：
Concurrency：多个线程操作相同的资源，保证线程安全、合理使用资源
High Concurrency：服务能同时处理很多请求，提高程序性能
性能优化：硬件、网络、系统架构、编程语言、数据结构、算法、数据库优化等等

知识技能：
总体架构：
  SpringBoot Maven JDK8 MySQL
基础组件：
  Mybatis Guava Lombok Redis Kafka
高级组件(类)：
  Joda-Time Atomic包 J.U.C AQS
  ThreadLocal RateLimiter Hystrix
  threadPool shardbatis curator 
  elastic-job...
```

- 1.基础知识与核心知识
```
并发、高并发相关概念
CPU多级缓存 缓存一致性 乱序执行优化
Java内存模型 JMM规定 抽象结构 同步操作与规则
并发优势与风险
并发模拟 Postman Jmeter 代码模拟验证
```  

- 2.并发及并发的线程安全处理
```
线程安全性：
  原子性  可见性 有序性
  atomic包
  CAS算法
  synchorized与Lock
  volaite
  happens-before
安全发布对象：
  安全发布方法
  不可变对象
  final关键字使用
  不可变方法
线程安全手段：线程封闭 同步容器 并发容器
  堆栈封闭：局部变量
  ThreadLocal线程封闭
  JDBC的线程封闭
  线程不安全类与写法
  同步容器
  并发容器
  J.U.C
AQS等J.U.C组件
  CounDownLatch
  Semaphore
  CyclicBarrier
  ReentrantLock与锁
  Condition
  FutureTask
  Fork/Join框架
  BlockingQueue
线程调度：线程池
  new Thread弊端
  线程池的好处
  ThreadPoolExecutor
  Executor框架接口
线程安全补充内容
  死锁的产生与预防
  多线程并发最佳实践
  Spring的线程安全
  HashMap和ConcurrentHashMap深入理解
```

- 3.高并发处理的思路及手段
```
扩容：
  水平扩容
  垂直扩容
缓存：
  Redis
  Memcache
  Guava Cache
队列：
  Kafka
  RabbitMQ
  RocketMQ
  使用队列的关注点
应用拆分：
  服务化Dubbo与微服务
  Spring Cloud技术栈
限流：
  Guava RateLimiter的介绍与使用
  常用限流算法
  自己实现分布式限流
服务降级与服务熔断
  服务降级的多种选择
  Hystrix介绍及使用
数据库切库、分库、分表
  介绍切库、分表
  支持多数据源的原理及实现
高可用的一些手段：
  任务调度分布式elastic-job
  主备curator的实现
  监控报警机制
```