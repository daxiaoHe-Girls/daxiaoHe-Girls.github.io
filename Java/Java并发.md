# 0.  目录

![目录](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_%E5%B9%B6%E5%8F%91/%E7%9B%AE%E5%BD%95.jpg)

# 1.  Java并发基础
（并发、线程）

![1.1](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_%E5%B9%B6%E5%8F%91/1.1.jpg)

![1.2.1](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_%E5%B9%B6%E5%8F%91/1.2.1.jpg)

![1.2.2](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_%E5%B9%B6%E5%8F%91/1.2.2.jpg)

### 补充
&nbsp;&nbsp;&nbsp;&nbsp;
sleep不放弃对象锁，wait放弃锁

### 线程的状态
- 初始状态（NEW）

> 初始状态，线程被创建，但是还没有调用start方法

- 运行状态（RUNNABLE）

> 运行状态，包括就绪和运行两种状态

- 等待状态（WAITING）

> 等待状态，该状态表示当前线程需要等待其他线程作出一些特定动作

- 超时等待状态（TIMED_WAITING）

> 超时等待状态，不同于waiting，可以在指定时间内自行返回

- 阻塞状态（BLOCKED）

> 表示线程阻塞于锁

- 终止状态（TERMINATED）

> 表示当前线程已经执行完毕

### 进程的状态
- 就绪状态

> 进程已经获得除处理机以外的资源，等待分配处理机资源

- 运行状态

> 占用处理机资源运行，出于此状态的进程数小于等于CPU数

- 阻塞状态

> 进程等待某种条件，在条件未满足之前无法执行


# 2.  Java并发机制
（原子操作、volatile、synchronized，final，三大性质）

![2](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_%E5%B9%B6%E5%8F%91/2.jpg)

# 3.  Java内存模型
（抽象结构、重排序、happens-before、双重检查锁定、8个原子操作、三大特性）

![3.1](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_%E5%B9%B6%E5%8F%91/3.1.jpg)

![3.2](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_%E5%B9%B6%E5%8F%91/3.2.jpg)

# 4.  线程间通信

![4](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_%E5%B9%B6%E5%8F%91/4.jpg)

# 5.  Lock体系
（Lock，AQS，ReentrantLock，ReentantLockReadWriteLock，Condiction，LockSupport）

![5.1](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_%E5%B9%B6%E5%8F%91/5.1.jpg)

![5.2](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_%E5%B9%B6%E5%8F%91/5.2.jpg)

# 6.  Java并发容器
（ConcurrentHashMap，CopyOnWriteArrayList，ThreadLocal，BlockingQueue，ConcurrentLinkedQueue，ForkJoin）

![6.1](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_%E5%B9%B6%E5%8F%91/6.1.jpg)

![6.2](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_%E5%B9%B6%E5%8F%91/6.2.jpg)

![6.3](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_%E5%B9%B6%E5%8F%91/6.3.jpg)

### 工作窃取算法
&nbsp;&nbsp;&nbsp;&nbsp;
指某个线程可以从其他队列里窃取任务来执行。被窃取任务的线程永远从双端队列的头部拿任务执行，而窃取任务的线程永远从双端队列的尾部拿任务执行。

# 7，8.  原子操作类+Java并发工具

![7](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_%E5%B9%B6%E5%8F%91/7.jpg)


# 9，10.  线程池+Executor体系

![9](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_%E5%B9%B6%E5%8F%91/9.jpg)
