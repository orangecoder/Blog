title: Java 开发总结
date: 2016-03-31 15:24:21
tags:
---

### 1. [Java 多线程总结](http://blog.csdn.net/kimylrong/article/details/17716175)
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

### 3. Java 异常总结
* Java 异常的基类为 java.lang.Throwable，java.lang.Error 和 java.lang.Exception 继承 Throwable，RuntimeException 和其它的 Exception 等继承 Exception，具体的 RuntimeException 继承 RuntimeException
* 错误和异常的区别(Error vs Exception) 
java.lang.Error: Throwable 的子类，用于标记严重错误。合理的应用程序不应该去 try/catch 这种错误。绝大多数的错误都是非正常的，就根本不该出现的。
java.lang.Exception: Throwable 的子类，用于指示一种合理的程序想去 catch 的条件。即它仅仅是一种程序运行条件，而非严重错误，并且鼓励用户程序去 catch 它。
* Error 和 RuntimeException 及其子类都是未检查的异常（unchecked exceptions），而所有其他的 Exception 类都是检查了的异常（checked exceptions）
checked exceptions: 通常是从一个可以恢复的程序中抛出来的，并且最好能够从这种异常中使用程序恢复。比如 FileNotFoundException, ParseException 等。检查了的异常发生在编译阶段，必须要使用 try…catch（或者 throws）否则编译不通过。
unchecked exceptions: 通常是如果一切正常的话本不该发生的异常，但是的确发生了。发生在运行期，具有不确定性，主要是由于程序的逻辑问题所引起的。比如 ArrayIndexOutOfBoundException, ClassCastException 等。从语言本身的角度讲，程序不该去 catch 这类异常，虽然能够从诸如 RuntimeException 这样的异常中 catch 并恢复，但是并不鼓励终端程序员这么做，因为完全没要必要。因为这类错误本身就是 bug，应该被修复，出现此类错误时程序就应该立即停止执行。 因此，面对 Errors 和 unchecked exceptions 应该让程序自动终止执行，程序员不该做诸如 try/catch 这样的事情，而是应该查明原因，修改代码逻辑。
* RuntimeException：RuntimeException 体系包括错误的类型转换、数组越界访问和试图访问空指针等等。处理 RuntimeException 的原则是：如果出现 RuntimeException，那么一定是程序员的错误。例如，可以通过检查数组下标和数组边界来避免数组越界访问异常。其他（IOException等等）checked异常一般是外部错误，例如试图从文件尾后读取数据等，这并不是程序本身的错误，而是在应用环境中出现的外部错误。

### 4. [Java 的 IO 操作总结](http://blog.csdn.net/zzp_403184692/article/details/8057693)
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

1. 缓冲流（BufferedInPutStream/BufferedOutPutStream和BufferedWriter/BufferedReader）他可以提高对流的操作效率。  
2. 转换流（InputStreamReader/OutputStreamWriter）
3. 数据流（DataInputStream/DataOutputStream）
4. 打印流（PrintStream/PrintWriter）
5. 对象流（ObjectInputStream/ObjectOutputStream）


### 5. [抽象类和接口联系与区别](http://www.cnblogs.com/azai/archive/2009/11/10/1599584.html)
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


### 6. [精选30道 Java 笔试题解答](http://www.cnblogs.com/lanxuezaipiao/p/3371224.html)


