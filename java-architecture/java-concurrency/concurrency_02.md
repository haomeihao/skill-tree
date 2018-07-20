#### 并发及并发的线程安全处理
``` 
1.线程安全性：原子性 可见性 有序性
2.安全发布对象：
```

- 1.线程安全性
``` 
原子性：互斥，同一时刻只能有一个线程来对它进行操作 atomic包、锁
可见性：一个线程对主内存的修改可以及时的被其他线程观察到
有序性：一个线程观察其他线程中的指令执行顺序，由于指令重排序
  的存在，该观察结果一般杂乱无序。
  
原子性：1.JDK的Atomic包
  原理CAS：Unsafe.compareAndSwapInt
  AtomicInteger -> count.incrementAndGet 
  -> Unsafe.getAndAddInt -> Unsafe.compareAndSwapInt(native修饰的方法)
  
  native方法：在编程领域, JNI (Java Native Interface,Java本地接口)是一种编程框架,
  使得Java虚拟机中的Java程序可以调用本地应用/或库,也可以被其他程序调用。 
  本地程序一般是用其它语言（C、C++或汇编语言等）编写的, 并且被编译为基于本机硬件和操作系统的程序。
  
  AtomicLong, LongAdder
  
  AtomicReference的compareAndSet
  AtomicIntegerFieldUpdater
    操作一个类对象的属性 必须被volatile修饰 且不能是static属性
    
  AtomicBoolean：
    保证某个逻辑执行一次
原子性：2.锁
  synchronized：依赖JVM
  Lock：依赖特殊的CPU指令，代码实现：ReentrantLock
  
  synchronized：
    修饰代码块 {} 作用于调用对象
    修饰方法 作用于实例调用对象
    修饰静态方法 作用于类对象
    修饰类 作用于类对象
    
原子性：3.对比  
  synchronized：不可中断锁，适合竞争不激烈，可读性好  
  Lock：可中断锁，多样化同步，竞争激烈时能维持常态
  Atomic：竞争激烈时能维持常态，比Lock性能好；只能同步一个值

可见性：
  导致共享变量在线程间不可见的原因：
    线程交叉执行
    重排序结合线程交叉执行
    共享变量更新后的值没有在工作内存与主内存间及时更新
  1.synchronized：
    线程解锁前，必须把共享变量的最新值刷新到主内存
    线程加锁前，将清空工作内存中共享变量的值，从而使用共享变量时需要从主内存中
      重新获取最新的值
  2.对volatile：线程不安全
    通过加入内存屏障和禁止重排序优化来实现
    对volatile变量写操作时，在写操作后加入一条store屏障，将本地
      内存中的共享变量值刷新到主内存
    对volatile变量读操作时，在读操作前加入一条load屏障，从主内存中
      读取共享变量
    适合于状态标志变量
    
有序性：
  JMM中，允许编译器和处理器对指令进行重排序，重排序不会影响单线程，但会影响
    多线程并发执行的正确性。
  volatile
  synchronized
  Lock
  happens-before原则：8个原则
    
线程安全性总结：
  原子性：atomic包、CAS算法、synchronized、Lock
  可见性：synchronized、volatile
  有序性：happens-before原则
```

- 2.安全发布对象
``` 
安全发布对象四种方法：
  通过单列举例，实现单例有三种安全方法：
  1.饿汉模式：类装载就初始化 内存浪费 -> 不太推荐
  2.懒汉模式：方法加synchronized 性能太低 -> 极不推荐
  3.懒汉模式：变量加volatile 双重同步锁单例模式 -> 目前在用
  4.枚举模式：最安全的 JVM只会初始化一次 -> 非常推荐
  推荐：枚举模式

不可变对象:  
  final关键字：类、方法、变量
  修饰类：不能被继承 String类
  修饰方法：1.锁定方法不能被继承类修改 2.效率
  修饰变量：基本数据类型变量、引用类型变量
    修饰引用类型变量：不允许指向别的对象，但值可以修改。
    java.util.Collections.unmodifiableXXX 值不可以修改
    (guava)com.google.common.collect.ImmutableXXX.of() 值不可以修改
```

- 3.线程安全手段
``` 
1.线程封闭：
  Ad-hoc：程序控制实现，最糟糕
  堆栈封闭：局部变量、无并发问题
  ThreadLocal 线程封闭：特别好的封闭方法
    Map.put("线程名", "线程局部变量")
    Spring的事务管理器中，对数据源获取的Connection放入了ThreadLocal中。

2.线程不安全类与写法
  StringBuilder -> StringBuffer
  SimpleDateFormat -> JodaTime(joda-time) 保证堆栈封闭就没事
  ArrayList,HashSet,HashMap...Collections
  先检查再执行(特别注意线程不安全的写法)：if(condition(a)) {handle(a);}
  
3.同步容器
  ArrayList -> Vector,Stack(继承Vector)
  HashMap -> HashTable(key,value不能为null)
  Collections.synchronizedXXX(List,Set,Map)
  三种循环方法：
    foreach -> list.remove 异常ConcurrentModificationException
    iterator -> list.remove 异常ConcurrentModificationException
                iterator.remove() 正常 
    fori -> list.remove 正常

4.并发容器J.U.C
  线程安全的容器：
  1.ArrayList -> CopyOnWriteArrayList
    CopyOnWriteArrayList缺点：消耗内存 导致YGC、FGC，适合读多写少
    设计思想：读写分离、最终一致性、使用时另外开辟空间(版本控制)
  2.HahsSet -> CopyOnWriteArraySet
    设计思想和CopyOnWriteArrayList一样
    TreeSet -> ConcurrentSkipListSet
    自然排序 
  3.HashMap -> ConcurrentHashMap 
    ConcurrentHashMap不允许null 性能高
    TreeMap -> ConcurrentSkipListMap
    key是有序的，支持更高的并发
    
    【CopyOnWriteArrayList -> ArrayList 重点】
    
    【CopyOnWriteArraySet -> HashSet 重点】
    ConcurrentSkipListSet -> TreeSet
 
    【ConcurrentHashMap -> HashMap 重点】
    ConcurrentSkipListMap -> TreeMap
    
    CopyOnWriteArrayList、CopyOnWriteArraySet
      设计思想：读写分离、最终一致性、使用时另外开辟空间(版本控制)
      适合：读多写少 缺点：消耗内存 导致YGC、FGC
      CopyOnWriteArraySet.add就是CopyOnWriteArrayList.add之前加上是否存在的判断
    ConcurrentHashMap
      分段锁
```

- 4.AQS等J.U.C组件
``` 
AbstractQueuedSynchronizer-AQS
J.U.C大大提高了并发的性能，即高并发
AQS是J.U.C的核心

Sync queue：head tail 双向链表
Condition queue：单项链表

使用Node实现FIFO队列，可以用于构建锁或者其他同步装置的基础框架
利用了一个int类型表示状态
使用方法是继承
子类通过继承并通过实现它的方法管理其状态{acquire和release}的方法操作状态
可以同时实现排它锁和共享锁模式(独占、共享)

AQS抽象队列同步组件：
  1.CountDownLatch
    同步辅助类，计数器减countDown() 阻塞直到0await() 不能重置
    父类等待子类们的处理完再执行
    countDownLatch.countDown(); //-1
    countDownLatch.await(); //阻塞到0
    countDownLatch.await(10, TimeUnit.MILLISECONDS); //阻塞到设置时长
  2.Semaphore
    同步辅助类，信号量，控制并发访问的线程数量
    仅能提供有限访问资源，如对数据库连接数做保护。
    semaphore.acquire(); //获取一个许可
    semaphore.release(); //释放一个许可
    semaphore.acquire(3); //获取多个许可
    semaphore.release(3); //释放多个许可
    semaphore.tryAcquire() //尝试获取一个许可
    semaphore.tryAcquire(5, TimeUnit.SECONDS) //尝试获取一个许可直到设置时长
  3.CyclicBarrier【很强大】
    同步辅助类，计数器加await()加1 初始0阻塞加到设置的值 可以重置
    和 CountDownLatch 区别：
      1).CyclicBarrier 的计数器可以通过reset()重置
      2).CountDownLatch是一个或多个线程等待其他线程的关系；
         CyclicBarrier是多个线程之间相互等待直到所有的线程都满足条件后再执行后续操作，
         能处理更复杂的任务。
    barrier.await(); // 阻塞直到计数器加到设置值，然后reset()为0 重新开始
    new CyclicBarrier(5); // 设置为5
    new CyclicBarrier(5, () -> {
        log.info("callback is running");
    }); // 构造函数，Runable条件满足优先执行
  4.ReentrantLock & Condition
    锁：synchronized、J.U.C里的锁ReentrantLock
    1.ReentrantLock(可重入锁)与synchronized区别：
      1)可重入性：区别不大，加锁计数器+1，直到计数器为0才释放锁
      2)锁的实现：synchronized是JVM(看不到)，ReentrantLock是JDK(代码可看)
      3)性能区别：之前synchronized比ReentrantLock差很多，自从JDK1.6引入了
        偏向锁，轻量级锁，自旋锁，现在更建议使用synchronized因为写法更简单，这些
        优化就是借鉴了ReentrantLock的CAS原理
      4)功能区别：
        便利性：synchronized更便利，ReentrantLock要在finally中释放锁
        锁粒度和灵活度：ReentrantLock优于synchronized
      独有功能：
        1)可以指定是否公平锁
        2)提供了一个Condition类，可以分组唤醒需要唤醒的线程
        3)提供能够中断等待锁的线程的机制，lock.lockInterruptibly()
    2.ReentrantLock原理：
        自旋锁：循环调用CAS来加锁
      默认非公平锁(性能高)  
    3.ReentrantReadWriteLock：
        悲观锁，想获取写锁，不允许有任何读锁，
        读锁是一个支持重进入的共享锁可多线程，写锁是一个支持重进入的排它锁只能单线程。
        读多写少，会造成饥饿，要写但是得不到锁。
    4.StampedLock
        乐观锁，对吞吐量优化很大
    总结：synchronized会自动解锁；Lock锁使用不当，会造成死锁。 
    5.Condition：
      reentrantLock.newCondition();
      condition.await(); //释放锁，线程1加入AQS等待队列
      condition.signalAll(); //AQS等待队列中线程1被唤醒
  5.FutureTask
    任务执行完得到任务执行结果
    Callable：call() 有返回值并且能抛出异常
    Runnable：run()
    Future接口 .get()
    FutureTask类
    
    ExecutorService executorService = Executors.newCachedThreadPool();
    Future<String> future = executorService.submit(new MyCallable());
    String result = future.get(); // 阻塞直到线程任务执行结束
    
    FutureTask<String> futureTask = new FutureTask(new MyCallable());
    new Thread(futureTask).start();
    String result = futureTask.get(); // 阻塞直到线程任务执行结束
    
  6.Fork/Join框架（MapReduce）
    双端链表实现双端队列
    ForkJoinPool
    ForkJoinTask
    //TODO
  7.BlockingQueue接口
    应用场景：生产者与消费者问题
    实现类：
      ArrayBlockingQueue：数组实现 有界队列
      DelayQueue：
      LinkedBlockingQueue：链表实现 无界队列
      PriorityBlockingQueue：优先级队列
      SynchronousQueue：同步队列 无界非缓存队列
```

- 5.线程池【很重要】
``` 
好处：
  1).重用存在的线程，减少对象创建、消亡的开销，性能佳
  2).可有效控制最大并发线程数，提高系统资源利用率，同时可
    以避免过多资源竞争，避免阻塞
  3).提供定时执行、定期执行、单线程、并发数控制等功能。
线程池：
  java.util.concurrent.ThreadPoolExecutor构造方法参数：
    corePoolSize：核心线程数量
    maximumPoolSize：线程最大线程数
    keepAliveTime：线程没有任务执行时最多保持多久时间终止
    unit：keepAliveTime的时间单位
    workQueue：阻塞队列，存储等待执行的任务，很重要，会对
      线程池运行过程产生重大影响。
    threadFactory：线程工厂，用来创建线程
    rejectHandler：当拒绝处理任务时的策略：
      1).CallerRunsPolicy
      2).AbortPolicy
      3).DiscardPolicy
      4).DiscardOldestPolicy
  线程池状态：
    1).Running
    2).Shutdown
    3).Stop
    4).Tidying
    5).terminated
  方法：
    execute()：提交任务，交给线程池执行
    shumit()：提交任务，能够返回执行结果execute+Future
    shutdown()：关闭线程池，等待任务都执行完
    shutdownNow()：关闭线程池，不能带任务执行完
    getTaskCount()：线程池已执行和未执行的任务总数
    getCompletedTaskCount()：已完成的任务数量
    getPoolSize()：线程池当前的线程数量
    getActiveCount()：当前线程池中正在执行任务的线程数量
  Executor接口：
    Executors.newCachedThreadPool
    Executors.newFixedThreadPool
    Executors.newScheduledThreadPool
    Executors.newSingleThreadExecutor
  合理配置：
    CPU密集型任务，参考值设置为NCPU+1
    IO密集型任务，参考值设置为2*NCPU  
```

- 6.J.U.C总结：
``` 
  1).atomic包 原子类
  2).locks包 锁
  3).tools 线程同步辅助类
  4).collections 并发容器
  5).executor 线程池
```

- 7.并发拓展【重点中的重点】
``` 
  1).死锁：你等我，我等你
    发生条件：
      互斥条件
      请求和保持条件
      不剥夺条件
      环路等待条件
  2).多线程并发最佳实践【重点总结】
    使用本地变量 
      局部变量堆栈封闭【静态(类)变量、实例(成员)变量、局部(本地)变量】
    使用不可变类immutable 
      final final只能保证指针改变，不能保证引用变量不被修改
      Collections.unmodifiableXXX
      com.google.common.collect.ImmutableXXX.of
      String不可变
    最小化锁的作用域范围  
    使用线程池的Executor，而不是直接new Thread()
    宁可使用同步也不要使用线程的wait和notify，
      如CountDownLatch,Semaphore,CyclicBarrier这些同步工具
    使用BlockingQueue实现生产者-消费者模式【最佳实践】
      ArrayBlockingQueue：数组实现 有界队列
      DelayQueue：
      LinkedBlockingQueue：链表实现 无界队列
      PriorityBlockingQueue：优先级队列
      SynchronousQueue：同步队列 无界非缓存队列
    使用并发集合而不是加了锁的同步集合
      【CopyOnWriteArrayList -> ArrayList 重点】
      【CopyOnWriteArraySet -> HashSet 重点】
       ConcurrentSkipListSet -> TreeSet
      【ConcurrentHashMap -> HashMap 重点】
       ConcurrentSkipListMap -> TreeMap
    使用Semaphore创建有界的访问【限制并发线程数】
    宁可使用同步代码块，也不使用同步方法
    避免使用静态变量，如必须使用最好加上final关键字使之不可变
  3).Spring与线程安全  
```

















