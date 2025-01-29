# JUC并发编程



## 创建线程的三种方法

**1. Thread**

- 继承Thread类，重写`run()`方法，调用`start()`方法启动线程

**2. Runnable**

- 实现Runnable接口，重写`run()`方法，将Runnable对象作为参数传递给Thread对象，调用start()方法启动线程

**3.FutureTask**

- 实现Callable接口，重写`call()`方法，然后创建FutureTask对象，参数为Callable对象；然后创建Thread对象，参数为FutureTask对象（因为FutureTask继承了Runnable接口），调用`start()`方法启动线程



## 对线程安全的理解

**1. 原子性**

- 确保当某个线程修改共享变量时，没有其他线程可以同时修改这个变量，即这个操作是不可分割的。

**2. 可见性**

- 确保一个线程对共享变量的修改可以立即被其他线程看到。

3.**活跃性**

- 要确保线程不会因为死锁、饥饿、活锁等问题导致无法继续执行。



## 进程和线程的区别

**进程**：是操作系统分配资源的最小单位

**线程**：是操作系统进行调度的最小单位。多个线程可以共享同一个进程的资源，如内存；每个线程都有自己独立的栈和寄存器。



## 线程的共享内存

Java中采取共享内存的方式实现线程之间的通信。

线程 A 与线程 B 之间如要通信的话，必须要经历下面 2 个步骤：

- 线程 A 把本地内存 A 中的共享变量副本刷新到主内存中。
- 线程 B 到主内存中读取线程 A 刷新过的共享变量，再同步到自己的共享变量副本中。

![深入浅出 Java 多线程：线程间通信](https://cdn.tobebetterjavaer.com/stutymore/javathread-20240315111130.png)



### 启动一个Java程序里面有哪些线程

**1. main主线程**：程序开始执行的入口

**2. 垃圾回收线程**：后台线程，负责回收不再使用的对象

**3. 编译器线程**：在及时编译中（JIT），负责把一部分热点代码编译后放到 codeCache 中，以提升程序的执行效率。



## start()和run()的区别

- 当调用`start()`方法时，会启动一个新的线程，并让这个新线程调用`run()`方法。

- 如果直接调用`run()`方法，那么`run()`方法就在当前线程中运行，没有新的线程被创建，也就没有实现多线程的效果。

![三分恶面渣逆袭：start方法](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javathread-5.png)

## 线程有哪些常用的调度方法

![三分恶面渣逆袭：线程常用调度方法](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javathread-6.png)

`yield()`：Thread 类中的静态方法，当一个线程调用 yield 方法时，实际是在暗示线程调度器，当前线程请求让出自己的 CPU（不会释放锁），但是线程调度器可能会“装看不见”忽略这个暗示。



## interrupt()和stop()的区别

`interrupt()`

- 用于请求一个线程中断（设置中断标志为 `true`），并不强制停止线程的运行。

`stop()`

- 强制停止一个线程的运行，无论线程当前处于什么状态。
- 已废弃，不安全



## 线程状态

**操作系统层面（5种状态）**

- 初始状态
- 可运行状态
- 运行状态
- 阻塞状态
- 终止状态

**Java层面（6种状态）**

![三分恶面渣逆袭：Java线程状态变化](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javathread-7.png)

- NEW
- RUNNABLE：包含操作系统层面的【可运行状态】、【运行状态】和【阻塞状态】
- BLOCKED：如没抢到synchronized的锁
- WAITING：join()、wait()，需要其他线程显式唤醒
- TIMED_WAITING：wait(1000)、sleep(1000)
- TERMINATED



## 什么是线程上下文切换

**上下文切换：**CPU 资源的分配采用了时间片轮转也就是给每个线程分配一个时间片，线程在时间片内占用 CPU 执行任务。当线程使用完时间片后，就会处于就绪状态并让出 CPU 让其他线程占用



## 什么是守护线程

Java中线程分为两类：**守护线程**、**用户线程**

- 守护线程的生命周期间接依赖用户线程，如果所有用户线程都结束了，守护线程会自动终止，JVM 退出。守护线程的生命周期完全依赖于 JVM 的运行状态。

- 守护线程一般用于后台服务，比如垃圾回收线程、线程池管理线程等。



## 线程之间有哪些通信方式

**1. 使用共享对象**

- volatile
- synchronized

**2. 使用wait()和notify()**

**3. 使用 Exchanger**

- 一个线程调用 `exchange()` 方法，将数据传递给另一个线程，同时接收另一个线程的数据。

**4.使用 CompletableFuture**



## sleep()和wait()的区别

**锁行为不同**

- 如果一个线程在持有某个对象的锁时调用了 sleep，它在睡眠期间仍然会持有这个锁。
- 当线程执行 wait 方法时，必须拥有锁，它会释放它持有的那个对象的锁，这使得其他线程可以有机会获取该对象的锁

**使用条件不同**

- `sleep()` 方法可以在任何地方被调用。
- `wait()` 方法必须在同步代码块或同步方法中被调用

**唤醒方式不同**

- 调用 sleep 方法后，线程会进入 TIMED_WAITING 状态（定时等待状态），即在指定的时间内暂停执行。当指定的时间结束后，线程会自动恢复到 RUNNABLE 状态（就绪状态），等待 CPU 调度再次执行。
- 调用 wait 方法后，线程会进入 WAITING 状态（无限期等待状态），直到有其他线程在同一对象上调用 notify 或 notifyAll，线程才会从 WAITING 状态转变为 RUNNABLE 状态，准备再次获得 CPU 的执行权。



## Synchronized锁住的是什么

synchronized是**基于Monitor实现**的。

实例对象结构里有对象头，对象头里面有一块结构叫 Mark Word，Mark Word 指针指向了**Monitor**。

**会牵扯到操作系统层面**

synchronized 升级为重量级锁时，依赖于操作系统的互斥量（mutex）来实现，mutex 用于保证任何给定时间内，只有一个线程可以执行某一段特定的代码段



## Synchronized怎么保证可见性、有序性、可重入

**可见性**

- 线程加锁前，将清空工作内存中共享变量的值，从而使用共享变量时需要从主内存中重新读取最新的值
- 线程加锁后，其它线程无法获取主内存中的共享变量
- 线程解锁前，必须把共享变量的最新值刷新到主内存中



**有序性**

- synchronized保证顺序性是指的将并发执行变成了串行，同一时刻代码是单线程执行的，但并不能保证内部指令重排序问题
- 因为 as-if-serial 语义的存在，单线程的程序能保证最终结果是有序的，但是不保证不会指令重排



**可重入**

- synchronized 之所以支持可重入，是因为 Java 的对象头包含了一个 Mark Word，用于存储对象的状态，包括锁信息。

- 当一个线程获取对象锁时，JVM 会将该线程的 ID 写入 Mark Word，并将锁计数器设为 1。

  如果一个线程尝试再次获取已经持有的锁，JVM 会检查 Mark Word 中的线程 ID。如果 ID 匹配，表示的是同一个线程，锁计数器递增。

  当线程退出同步块时，锁计数器递减。如果计数器值为零，JVM 将锁标记为未持有状态，并清除线程 ID 信息。

  

## Synchronized优化原理

![三分恶面渣逆袭：Mark Word变化](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javathread-34.png)

![三分恶面渣逆袭：锁升级简略过程](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javathread-36.png)

### 1. 轻量级锁

### 2. 锁膨胀

![image-20250126232145374](https://github.com/user-attachments/assets/4b71e771-e9b5-4d13-81f9-9794447ffa6b)

![image-20250126232331090](https://github.com/user-attachments/assets/7f690957-4562-4041-9f95-1bc05d9e41f8)


### 3. 自旋优化

当线程尝试获取轻量级锁失败时，它会进行自旋，即循环检查锁是否可用，以避免立即进入阻塞状态。如果当前线程自旋成功（即持锁线程已经退出同步块，释放了锁），这时当前线程就可以避免阻塞。

![image-20250126233420970](https://github.com/user-attachments/assets/d6ec6358-1bc1-418a-a205-8675dc0f6a1a)

![image-20250126233439471](https://github.com/user-attachments/assets/07464428-0a50-46d1-8b2d-ba8f67fcf957)


### 4. 偏向锁
![image-20250127134217619](https://github.com/user-attachments/assets/87c0052d-a8a1-402a-a4b0-01f203cea2f1)


**偏向锁撤销**

- 在偏向锁状态下，调用`hashCode()`方法会会撤销偏向锁，改为轻量级锁
- 当有其它线程使用偏向锁对象时，会将偏向锁升级为轻量级锁

- 调用wait / notify

  

（ 如果另一个线程尝试获取这个已被偏向的锁，JVM 会检查当前持有偏向锁的线程是否活跃。如果持有偏向锁的线程不活跃，则可以将锁重偏向至新的线程；如果持有偏向锁的线程还活跃，则需要撤销偏向锁，升级为轻量级锁。）

**批量重偏向**

当撤销偏向锁阈值超过20次后，JVM会认为自己偏向错了，于是会在给这些对象加锁时重新偏向至加锁线程

**批量撤销**

当撤销偏向锁阈值超过40次时，JVM会认为不该偏向，于是整个类的所有对象都会变成不可偏向的。

### 5. 锁消除

如果只有一个线程可能用到锁，JIT即时编译会对代码进行优化，去掉锁。

### 6. 锁粗化

如果 JVM 检测到一系列连续的锁操作实际上是在单一线程中完成的，则会将多个锁操作合并为一个更大范围的锁操作，这可以减少锁请求的次数。



## ReentrantLock是什么

**可中断**（Synchronized不行）

​	`lock.lockInterruptibly()`

​	如果没有竞争此方法就获取lock对象锁

​	如果有竞争就进入阻塞队列，可以被其它线程用interrupt方法打断

**可以设置超时时间**（Synchronized不行）

​	`lock.tryLock(1, TimeUnit.SECONDS)`

​	返回值是boolean类型，获取锁成功为true

**可以设置公平锁**（Synchronized不行）

**支持多个条件变量**（Synchronized不行）

​	Condition A = ReentrantLock.newCondition();

​	A.await();

​	A.signal();

**与synchronized一样，都支持可重入**



## volatile原理

**1. 如何保证可见性**

- 对volatile变量的写指令后会加入**写屏障**
  - 写屏障保证在该屏障之前，对所有变量的改动都同步到主存当中
- 对volatiole变量的读指令前会加入**读屏障**
  - 读屏障保证在该屏障之后，对共享变量的读取加载的是主存中最新数据



**2. 如何保证有序性**

- 写屏障会确保指令重排序时，不会将写屏障之前饿代码排在写屏障之后
- 读屏障会确保指令重排序时，不会将读屏障之后的代码排在读屏障之前



**3. 不能解决指令交错**

![image-20250128213711292](https://github.com/user-attachments/assets/21d90c48-51be-4861-b0d3-3ff428950355)



**懒汉式单例模式**

```java
public final class Singleton {
    private Singleton(){}
    private static Singleton INSTANCE = null;
    
    public static Singleton getInstance() {
        if(INSTANCE == null){
            synchronized (Singleton.class){
                if(INSTANCE == null){
                    INSTANCE = new Singleton();
                }
            }
        }
        return INSTANCE;
    }
}
```

这段代码是**不正确**的，因为INSTANCE变量并没有完全被synchronized代码块包围住。且synchronized内部是有可能发生指令重排序的，即先给INSTANCE赋值，再调用`new Singletion()`方法初始化，这时有第二个线程在INSTANCE初始化之前就返回了INSTANCE。



```java
public final class Singleton {
    private Singleton(){}
    private static Singleton INSTANCE = null;
    
    public static Singleton getInstance() {
        if(INSTANCE == null){
            synchronized (Singleton.class){
                if(INSTANCE == null){
                    INSTANCE = new Singleton();
                }
            }
        }
        return INSTANCE;
    }
}

```

在INSTANCE变量上加上volatile防止重排序，通过写屏障保证初始化在赋值之前执行。



## ThreadLocal怎么实现的

ThreadLocal 的实现原理就是，每个线程维护一个 ThreadLocalMap，key 为 ThreadLocal 对象，value 为想要实现线程隔离的对象。

1、当需要存线程隔离的对象时，通过 ThreadLocal 的 set 方法将对象存入 Map 中。

2、当需要取线程隔离的对象时，通过 ThreadLocal 的 get 方法从 Map 中取出对象。

3、Map 的大小由 ThreadLocal 对象的多少决定。

![ThreadLocal 的结构](https://cdn.tobebetterjavaer.com/stutymore/javathread-20240407205747.png)

## ThreadLocal内存泄露是怎么回事

**什么是弱引用，什么是强引用**

![三分恶面渣逆袭：ThreadLocal内存分配](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javathread-14.png)

如果某个 ThreadLocal 变量在代码中不再有强引用（例如 threadLocal = null;），并且它的 key 是 **弱引用**（在 ThreadLocalMap 中），那么在下一次垃圾回收时，ThreadLocal 实例就会被回收。

在 ThreadLocal 被垃圾收集后，下一次访问 ThreadLocalMap 时，Java 会自动清理那些键为 null 的条目（参照源码中的 replaceStaleEntry 方法），这个过程会在执行 ThreadLocalMap 相关操作（如 `get()`, `set()`, `remove()`）时触发。



**内存泄露**

通常情况下，随着线程 Thread 的结束，其内部的 ThreadLocalMap 也会被回收，从而避免了内存泄漏。

但如果一个线程一直在运行，并且其 ThreadLocalMap 中的 Entry.value 一直指向某个强引用对象，那么这个对象就不会被回收，从而导致内存泄漏。当 Entry 非常多时，可能就会引发更严重的内存溢出问题。



## ThreadLocalMap的源码看过吗

**1. 元素数组**

一个 table 数组，存储 Entry 类型的元素，Entry 是 ThreaLocal 弱引用作为 key，Object 作为 value 的结构。

```java
private Entry[] table;
```



**2. 散列方法**

散列方法就是怎么把对应的 key 映射到 table 数组的相应下标，ThreadLocalMap 用的是哈希取余法，取出 key 的 threadLocalHashCode，然后和 table 数组长度减一&运算（相当于取余）。

```java
int i = key.threadLocalHashCode & (table.length - 1);
```

每创建一个 ThreadLocal 对象，它就会新增`0x61c88647`，这个值很特殊，它是**斐波那契数** 也叫 **黄金分割数**。`hash`增量为 这个数字，带来的好处就是 `hash` **分布非常均匀**。

```java
private static final int HASH_INCREMENT = 0x61c88647;

private static int nextHashCode() {
    return nextHashCode.getAndAdd(HASH_INCREMENT);
}
```



## 父子线程怎么共享数据

父线程能用 ThreadLocal 来给子线程传值吗？**不能。**





## 线程池状态有哪些

ThreadPoolExecutor使用int的高3位来表示线程池状态，低29位表示线程数量


![image-20250129105817900](https://github.com/user-attachments/assets/34d583d7-0734-44de-86ee-4a79b75201c0)



## 线程池参数有哪些

**corePoolSize**：核心线程数

**maximumPoolSize**: 最大线程数（救急线程 = maximumPoolSize - corePoolSize，当核心线程和阻塞队列都满了的时候，下一个来的任务交给**救急线程**运行，救急线程执行完了就会销毁，不会像核心线程一样保留）

**keepAliveTime**：生存时间 — 针对救急线程

**unit**：时间单位

**workQueue**：阻塞队列

**threadFactory**：线程工厂 — 为线程创建时起名字

**handler**：拒绝策略



## 线程池的拒绝策略有哪些

![image-20250129111236731](https://github.com/user-attachments/assets/e52ef73d-51d0-4978-b47e-d69cb0bdb97a)




## 线程池类型

**newFixedThreadPool**

```java
public static ExecutorService newFixedThreadPool(int nThreads) {
    return new ThreadPoolExecutor(
        nThreads, 
        nThreads, 
        0L, 
        TimeUnit.SECONDS, new LinkedBlockingQueue<Runnable>());
}
```

- 核心线程 = 最大线程数，没有救急线程，因此也无需超时时间
- 阻塞队列时无界的

适合于任务量已知，相对耗时的任务



**newCachedThreadPool**

```java
public static ExecutorService newCachedThreadPool() {
    return new ThreadPoolExecutor(
        0, 
        Integer.MAX_VALUE, 
        60L, 
        TimeUnit.SECONDS, 
        new SynchronousQueue<Runnable>());
}
```

- 核心线程数是0，最大线程数是Integer.MAX_VALUE，生存时间是60s

  - 全部都是救急线程

  - 救急线程可以无限创建

- 队列采用了SynchronousQueue实现特点是，没有容量，没有线程来取是放不进去的

适合任务数密集，但每个任务执行时间较短的情况



**newSingleThreadExecutor**

```java
public static ExecutorService newSingleThreadExecutor() {
    return new FinalizableDelegatedExecutorService(
    	new ThreadPoolExecutor(
            i, 
            1,
            0L,
            TimeUnit.MILLISECONDS,
            new LinkedBlockingQueue<Runnable>())
    );
}
```

- 线程数固定为1且始终为1，不能修改
- 任务数多于1时，会放入无界队列排队
- 还会新建一个线程，保证池的正常工作（始终保证有一个可用的线程）



## 线程池的shutdown()和shutdownNow()区别

**shutdown()**

- 线程池不能在接收新的任务
- 已经提交的任务（包括当前队列中的任务）仍然会执行，都执行完后线程池才真正关闭
- 正在执行的任务继续运行，不会被中断



**shutdownNow()**

- 线程池不能在接受新任务
- 当前队列中的会被直接返回，不执行
- 正在执行的任务会被`interrupt()`中断










## 参考资料

[黑马程序员JUC并发编程教程](https://www.bilibili.com/video/BV16J411h7Rd?spm_id_from=333.788.videopod.episodes&vd_source=40110d1722fdce829e736c2a08fb55ae)：

- P11 - 15 （创建线程）
- p44 - 46 （线程状态）
- p78 - 87 （Synchronized优化原理）
- p120 - 127 （ReentrantLock）
- p146 - 151 （volatile原理）
- P200 - 219 （从自定义线程池到ThreadPoolExecutor）
- P274 - 296 （ConcurrentHashMap）



[Java面渣](https://javabetter.cn/sidebar/sanfene/javathread.html#)

