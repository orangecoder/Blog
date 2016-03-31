title: Java 开发总结
date: 2016-03-31 15:24:21
tags:
---

### 1. [Java多线程总结](http://blog.csdn.net/kimylrong/article/details/17716175)
* Java 的线程有4种状态: 新建(New)、运行(Runnable)、阻塞(Blocked)、结束(Dead)
* Java 线程阻塞(Blocked)的类型:
|-- 调用 sleep 函数进入睡眠状态，Thread.sleep(1000) 或者 TimeUnit.SECONDS.sleep(1)，sleep 不会释放锁
|-- 等待(wait)某个事件，分为两种，(wait,notify,notifyAll)，(await, signal,signalAll), 必须在获取到锁的环境才能调用
|-- 等待锁，synchronized 和 lock 环境中，锁已经被别的线程拿走，等待获取锁
|-- IO阻塞(Blocked)，比如网络等待，文件打开，控制台读取。System.in.read()
* 线程驱动任务的处理，涉及到两个概念，线程(Thread)、以及任务(Task, Java里面叫Runnable)，推荐的做法是写一个类(class)实现 Runnable 接口
* Java有两种锁可供选择:
|-- 对象或者类(class)的锁, 每一个对象或者类都有一个锁。使用 synchronized 关键字获取。 synchronized 加到 static 方法上面就使用类锁，加到普通方法上面就用对象锁。
|-- 显示构建的锁(java.util.concurrent.locks.Lock)，调用 lock 的 lock 方法锁定关键代码。
* 可见性(visibility)是指多个线程之间看到完全相同的资源，任何线程对资源的修改，其它线程都能够实时看到。synchronized 能够保证资源的可见性 , volatile 关键字也能够保证资源的可见性
* Thread.yield() 释放当前线程的 cpu 时间，让同优先级以上的线程可以获取 cpu 
* thread.join() 让调用该方法的 thread 完成 run 方法里面的东西后， 再执行join()方法后面的代码
* wait notify notifyAll，这三个都是 Object 的方法，wait 用来阻塞(Block)当前线程，但是会释放对象的锁，notify 告诉线程分派器唤醒等待的某一个线程，notifyAll 会唤醒所有等待的线程，需要特别注意的是，这三个方法都需要在锁定的环境(synchronized)里面使用，否则编译通过，但是运行报错。wait notify notifyAll 与 synchronized 配对使用
* await signal signalAll 是java.util.concurrent.locks.Condition的三个方法，Condition 可以通过 Lock 获得 Condition condition = lock.newCondition()。从功能上来说，await signal signalAll与wait notify notifyAll应该是相同的，只是await signal signalAll 与 Condition 配对使用

### 2. [Java 常用集合总结](http://www.cnblogs.com/linjiqin/archive/2013/05/30/3107785.html)
* 线程安全就是说多线程访问同一代码，不会产生不确定的结果
* List 类和 Set 类
* HashMap 和 HashTable
* 线程安全集合类与非线程安全集合类
|-- LinkedList、ArrayList、HashSet 是非线程安全的，Vector 是线程安全的;
|-- HashMap 是非线程安全的，HashTable 是线程安全的;
|-- StringBuilder 是非线程安全的，StringBuffer 是线程安全的 
* 集合适用场景
|-- 对于查找和删除较为频繁，且元素数量较多的应用，Set 或 Map 是更好的选择；
|-- ArrayList 适用于通过为位置来读取元素的场景；
|-- LinkedList 适用于要头尾操作或插入指定位置的场景；
|-- Vector 适用于要线程安全的 ArrayList 的场景；
|-- Stack 适用于线程安全的LIFO场景；
|-- HashSet 适用于对排序没有要求的非重复元素的存放；
|-- TreeSet 适用于要排序的非重复元素的存放；
|-- HashMap 适用于大部分key-value的存取场景；
|-- TreeMap 适用于需排序存放的key-value场景。

### 3. [精选30道 Java 笔试题解答](http://www.cnblogs.com/lanxuezaipiao/p/3371224.html)


