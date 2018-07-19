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
  -> Unsafe.getAndAddInt -> Unsafe.compareAndSwapInt
  
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
    com.google.common.collect.ImmutableXXX.of() 值不可以修改
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
  SimpleDateFormat -> JodaTime
  ArrayList,HashSet,HashMap...Collections
```






















