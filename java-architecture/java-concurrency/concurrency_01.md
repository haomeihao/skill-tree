#### 基础知识与核心知识
``` 
并发、高并发相关概念
1.CPU多级缓存：缓存一致性 乱序执行优化
2.Java内存模型 JMM规定 抽象结构 同步操作与规则
3.并发优势与风险
4.并发模拟 Postman Jmeter 代码模拟验证
```

- 1.CPU多级缓存
``` 
主内存(RAM) -> 三级CPU缓存(高速缓存) -> CPU(CPU寄存器)
时间局部性：某个数据被访问，不久将来很可能再次被访问
空间局部性：某个数据被访问，与它相邻的数据很快也可能被访问

缓存一致性(MESI) 
用于保证多个CPU缓存之间缓存共享数据的一致性

乱序执行优化
```

- 2.Java内存模型
``` 
Java Memory Model, JMM
Heap -> 主内存
Thread Stack -> CPU缓存

同步8种操作
同步规则
主内存 -> save/load -> 工作内存(多个) -> 多个Java线程
主内存：(1)Lock (2)Read  (7)Write (8)Unlock
工作内存：(3)Load (4)Use  (5)Assign (6)Store
```

- 3.并发优势与风险
``` 
优势：速度(响应更快)、设计、资源利用
风险：安全性(共享数据与期望不相符)、活跃性(死锁)、性能
```

- 4.并发模拟
``` 
Postman
Apache Bench
Jmeter
代码：
  Semaphore 信号量 阻塞线程 控制并发数
    semaphore.acquire(); //拦截
    semaphore.release(); //通行
  CountDownLatch 计数器减 线程执行完操作
    countDownLatch.countDown(); //减1
    countDownLatch.await(); //阻塞直到计数器为0
```






















