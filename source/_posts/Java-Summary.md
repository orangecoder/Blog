title: Java 开发总结
date: 2016-03-31 15:24:21
tags:
---

### 1. [Java Thread 类总结](http://blog.csdn.net/kimylrong/article/details/17716175)
* Java 的线程有4种状态: 新建(New)、运行(Runnable)、阻塞(Blocked)、结束(Dead)
* Java 线程阻塞(Blocked)的类型:
1）调用 sleep 函数进入睡眠状态，Thread.sleep(1000) 或者 TimeUnit.SECONDS.sleep(1)，sleep 不会释放锁
2）等待(wait)某个事件，分为两种，(wait,notify,notifyAll)，(await, signal,signalAll), 必须在获取到锁的环境才能调用
3）等待锁，synchronized 和 lock 环境中，锁已经被别的线程拿走，等待获取锁
4）IO阻塞(Blocked)，比如网络等待，文件打开，控制台读取。System.in.read()
* 线程驱动任务的处理，涉及到两个概念，线程(Thread)、以及任务(Task, Java里面叫Runnable)，推荐的做法是写一个类(class)实现 Runnable 接口
* Java有两种锁可供选择:
1）对象或者类(class)的锁, 每一个对象或者类都有一个锁。使用 synchronized 关键字获取。 synchronized 加到 static 方法上面就使用类锁，加到普通方法上面就用对象锁。
2）显示构建的锁(java.util.concurrent.locks.Lock)，调用 lock 的 lock 方法锁定关键代码。
* 可见性(visibility)是指多个线程之间看到完全相同的资源，任何线程对资源的修改，其它线程都能够实时看到。synchronized 能够保证资源的可见性 , volatile 关键字也能够保证资源的可见性
* Thread.yield() 释放当前线程的 cpu 时间，让同优先级以上的线程可以获取 cpu 
* thread.join() 让调用该方法的 thread 完成 run 方法里面的东西后， 再执行join()方法后面的代码
* wait notify notifyAll，这三个都是 Object 的方法，wait 用来阻塞(Block)当前线程，但是会释放对象的锁，notify 告诉线程分派器唤醒等待的某一个线程，notifyAll 会唤醒所有等待的线程，需要特别注意的是，这三个方法都需要在锁定的环境(synchronized)里面使用，否则编译通过，但是运行报错。wait notify notifyAll 与 synchronized 配对使用
* await signal signalAll 是java.util.concurrent.locks.Condition的三个方法，Condition 可以通过 Lock 获得 Condition condition = lock.newCondition()。从功能上来说，await signal signalAll与wait notify notifyAll应该是相同的，只是await signal signalAll 与 Condition 配对使用

### 2. [Java Executors 类总结](http://shmilyaw-hotmail-com.iteye.com/blog/1765751)
* 为什么要用线程池
1）减少了创建和销毁线程的次数，每个工作线程都可以被重复利用，可执行多个任务（时间效率）
2）可以根据系统的承受能力，调整线程池中工作者线程的数目，防止消耗过多的内存 (空间效率)
* Executors 提供了创建以下几类线程池的方法
1）Single Thread Executor: 
创建的线程池只包含一个线程，所有提交到线程池的线程会按照提交的顺序一个接一个的执行。通过 Executors.newSingleThreadExecutor() 方法创建。这种线程池适用于我们只希望每次使用一个线程的情况。
2）Cached Thread Pool: 
线程池里会创建尽可能多的必须线程来并行执行。一旦前面的线程执行结束后可以被重复使用。当然，使用这种线程池的时候我们必须要小心。我们使用多线程的目的是希望能够提高并行度和效率，但是并不是线程越多就越好。如果我们设定的线程数目过多的时候，使用 Cached Thread Pool 并不是一个很理想的选择。因为一方面它占用了大量的线程资源，同时线程之间互相切换很频繁的时候也会带来执行效率的下降。它主要适用于使用的线程数目不多，但是对线程有比较灵活动态的要求。一般通过 Executors.newCachedThreadPool() 来创建。
3）Fix Thread Pool: 
线程池里会创建固定数量的线程。在线程都被使用之后，后续申请使用的线程都会被阻塞在那里。使用 Executors.newScheduledThreadPool() 创建。
4）Scheduled Thread Pool: 
线程池可以对线程执行的时间和顺序做预先指定。比如说要某些线程在某个时候才启动或者每隔多长时间就启动一次。有点像我们的 Timer Job。使用 Executors.newScheduledThreadPool()创建。
5）Single Thread Scheduled Pool: 
线程池按照指定的时间或顺序来启动线程池。同时线程池里只有一个线程。创建方法：Executors.newSingleThreadScheduledExecutor()
* Java 线程池和线程数的选择
1）CPU 密集
对于 CPU 密集类型的问题来说，更多只是 CPU 要做大量的运算处理。这个时候如果要保证系统最充分的利用率，我们最好定义的线程数量和系统的 CPU 数量一致。这样每个 CPU 都可以充分的运用起来，而且很少或者不会有线程之间的切换。在这种情况下，比较理想的线程数目是 CPU 的个数或者比 CPU个 数大1
2）IO 密集
对于 IO 密集型的问题来说，我们会发现由于 IO 经常会带来各种中断，并且 IO 的速度和 CPU 处理速度差别很大。因此，我们需要考虑到 IO 在整个运算时间中所占的比例。比如说我们有50%比例的时间是阻塞的，那么我们可以创建相当于当前 CPU 数目的两倍数的线程。假设我们知道阻塞的比例数的话，我们可以通过如下的一个简单关系来估算线程数的大小：
线程数 = 可用CPU个数 / （1 - 阻塞比例数）

### 3. [Java 常用集合总结](http://www.cnblogs.com/linjiqin/archive/2013/05/30/3107785.html)
* 线程安全就是说多线程访问同一代码，不会产生不确定的结果
* List 类和 Set 类
* HashMap 和 HashTable
* 线程安全集合类与非线程安全集合类
1）LinkedList、ArrayList、HashSet 是非线程安全的，Vector 是线程安全的;
2）HashMap 是非线程安全的，HashTable 是线程安全的;
3）StringBuilder 是非线程安全的，StringBuffer 是线程安全的 
* 集合适用场景
1）对于查找和删除较为频繁，且元素数量较多的应用，Set 或 Map 是更好的选择；
2）ArrayList 适用于通过为位置来读取元素的场景；
3）LinkedList 适用于要头尾操作或插入指定位置的场景；
4）Vector 适用于要线程安全的 ArrayList 的场景；
5）Stack 适用于线程安全的LIFO场景；
6）HashSet 适用于对排序没有要求的非重复元素的存放；
7）TreeSet 适用于要排序的非重复元素的存放；
8）HashMap 适用于大部分key-value的存取场景；
9）TreeMap 适用于需排序存放的key-value场景。

### 4. Java 异常总结
* Java 异常的基类为 java.lang.Throwable，java.lang.Error 和 java.lang.Exception 继承 Throwable，RuntimeException 和其它的 Exception 等继承 Exception，具体的 RuntimeException 继承 RuntimeException
* 错误和异常的区别(Error vs Exception) 
1）java.lang.Error: Throwable 的子类，用于标记严重错误。合理的应用程序不应该去 try/catch 这种错误。绝大多数的错误都是非正常的，就根本不该出现的。
2）java.lang.Exception: Throwable 的子类，用于指示一种合理的程序想去 catch 的条件。即它仅仅是一种程序运行条件，而非严重错误，并且鼓励用户程序去 catch 它。
* Error 和 RuntimeException 及其子类都是未检查的异常（unchecked exceptions），而所有其他的 Exception 类都是检查了的异常（checked exceptions）
1）checked exceptions: 通常是从一个可以恢复的程序中抛出来的，并且最好能够从这种异常中使用程序恢复。比如 FileNotFoundException, ParseException 等。检查了的异常发生在编译阶段，必须要使用 try…catch（或者 throws）否则编译不通过。
2）unchecked exceptions: 通常是如果一切正常的话本不该发生的异常，但是的确发生了。发生在运行期，具有不确定性，主要是由于程序的逻辑问题所引起的。比如 ArrayIndexOutOfBoundException, ClassCastException 等。从语言本身的角度讲，程序不该去 catch 这类异常，虽然能够从诸如 RuntimeException 这样的异常中 catch 并恢复，但是并不鼓励终端程序员这么做，因为完全没要必要。因为这类错误本身就是 bug，应该被修复，出现此类错误时程序就应该立即停止执行。 因此，面对 Errors 和 unchecked exceptions 应该让程序自动终止执行，程序员不该做诸如 try/catch 这样的事情，而是应该查明原因，修改代码逻辑。
* RuntimeException：RuntimeException 体系包括错误的类型转换、数组越界访问和试图访问空指针等等。处理 RuntimeException 的原则是：如果出现 RuntimeException，那么一定是程序员的错误。例如，可以通过检查数组下标和数组边界来避免数组越界访问异常。其他（IOException等等）checked异常一般是外部错误，例如试图从文件尾后读取数据等，这并不是程序本身的错误，而是在应用环境中出现的外部错误。

### 5. [Java 的 IO 操作总结](http://blog.csdn.net/zzp_403184692/article/details/8057693)
* Java 的 IO 操作中有面向字节( Byte )和面向字符( Character )两种方式
面向字节的操作为以8位为单位对二进制的数据进行操作，对数据不进行转换，这些类都是 InputStream 和 OutputStream 的子类。
面向字符的操作为以字符为单位对数据进行操作，在读的时候将二进制数据转为字符，在写的时候将字符转为二进制数据，这些类都是 Reader 和 Writer 的子类。

* IO 流主要可以分为节点流和处理流两大类

节点流类型

类型	             | 字符流	       | 字节流
-----------------|-----------------|-----------------
File(文件)        | FileReader      | FileInputStream
                  |FileWriter      | FileOutputSream
Memory Array     | CharArrayReader | ByteArrayInputStream
                 | CharArrayWriter | ByteArrayOutputSream
Memory String    | StringReader
                 | StringWriter
Pipe(管道)        | PipedReader     | PipedInputSream
                 | PipedWriter      | PipedOutputSream

处理流类型

1）缓冲流（BufferedInPutStream/BufferedOutPutStream和BufferedWriter/BufferedReader）他可以提高对流的操作效率。  
2）转换流（InputStreamReader/OutputStreamWriter）
3）数据流（DataInputStream/DataOutputStream）
4）打印流（PrintStream/PrintWriter）
5）对象流（ObjectInputStream/ObjectOutputStream）


### 6. [抽象类和接口联系与区别](http://www.cnblogs.com/azai/archive/2009/11/10/1599584.html)
abstract class 和 interface 是 Java 语言中的两种定义抽象类的方式，它们之间有很大的相似性。但是对于它们的选择却又往往反映出对于问题领域中的概念本质的理解、对于设计意图的反映是否正确、合理，因为它们表现了概念间的不同的关系. abstract class 表示的是 "is a" 关系，interface 表示的是 "like a" 关系
* 抽象类和接口都不能直接实例化，如果要实例化，抽象类变量必须指向实现所有抽象方法的子类对象，接口变量必须指向实现所有接口方法的类对象。
* 抽象类要被子类继承，接口要被类实现。
* 接口只能做方法申明，抽象类中可以做方法申明，也可以做方法实现
* 接口里定义的变量只能是公共的静态的常量，抽象类中的变量是普通变量。
* 抽象类里的抽象方法必须全部被子类所实现，如果子类不能全部实现父类抽象方法，那么该子类只能是抽象类。同样，一个实现接口的时候，如不能全部实现接口方法，那么该类也只能为抽象类。
* 抽象方法只能申明，不能实现。abstract void abc(); 不能写成abstract void abc(){}。
* 抽象类里可以没有抽象方法
* 如果一个类里有抽象方法，那么这个类只能是抽象类
* 抽象方法要被实现，所以不能是静态的，也不能是私有的。
* 接口可继承接口，并可多继承接口，但类只能单根继承。
特别是对于公用的实现代码，抽象类有它的优点。抽象类能够保证实现的层次关系，避免代码重复。然而，即使在使用抽象类的场合，也不要忽视通过接口定义行为模型的原则。从实践的角度来看，如果依赖于抽象类来定义行为，往往导致过于复杂的继承关系，而通过接口定义行为能够更有效地分离行为与实现，为代码的维护和修改带来方便。

### 7. [Java 线程锁机制：synchronized、Lock、Condition](http://blog.csdn.net/vking_wang/article/details/9952063)
* synchronized
1) 原子性
原子性意味着个时刻，只有一个线程能够执行一段代码
2) 可见性
确保释放锁之前对共享数据做出的更改对于随后获得该锁的另一个线程是可见的 
原理：当对象获取锁时，它首先使自己的高速缓存无效，这样就可以保证直接从主内存中装入变量。同样，在对象释放锁之前，它会刷新其高速缓存，强制使已做的任何更改都出现在主内存中。 这样，会保证在同一个锁上同步的两个线程看到在 synchronized 块内修改的变量的相同值。类似的规则也存在于 volatile 变量上。volatile 只保证可见性，不保证原子性。
3）何时要同步
一致性同步：当修改多个相关值时，您想要其它线程原子地看到这组更改，要么看到全部更改，要么什么也看不到。
4）synchronize 的限制
它无法中断一个正在等候获得锁的线程；
也无法通过投票得到锁，如果不想等下去，也就没法得到锁；
同步还要求锁的释放只能在与获得锁所在的堆栈帧相同的堆栈帧中进行，多数情况下，这没问题（而且与异常处理交互得很好），但是，确实存在一些非块结构的锁定更合适的情况
* ReentrantLock
java.util.concurrent.lock 中的 Lock 框架是锁定的一个抽象，它允许把锁定的实现作为 Java 类，而不是作为语言的特性来实现。这就为 Lock 的多种实现留下了空间，各种实现可能有不同的调度算法、性能特性或者锁定语义。
* 读写锁 ReadWriteLock
与互斥锁定相比，读-写锁定允许对共享数据进行更高级别的并发访问。虽然一次只有一个线程（writer 线程）可以修改共享数据，但在许多情况下，任何数量的线程可以同时读取共享数据（reader 线程）
* 线程间通信 Condition
Condition 可以替代传统的线程间通信，用 await() 替换 wait()，用 signal() 替换 notify()，用 signalAll() 替换 notifyAll()。




### 8. [精选30道 Java 笔试题解答](http://www.cnblogs.com/lanxuezaipiao/p/3371224.html)


