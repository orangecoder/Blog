title: Java Concurrency Utilities
date: 2016-05-11 14:21:27
tags:
---
* 阻塞队列 BlockingQueue

BlockingQueue 通常用于一个线程生产对象，而另外一个线程消费这些对象的场景
![](/image/java-concurrency-1.png)

BlockingQueue 具有 4 组不同的方法用于插入、移除以及对队列中的元素进行检查。如果请求的操作不能得到立即执行的话，每个方法的表现也不同。这些方法如下：

操作    | 抛异常	     | 特定值	     | 阻塞	     | 超时
-------|-------------|-----------|-----------|----------
插入	   | add(o)	     | offer(o)	 | put(o)    | offer(o, timeout, timeunit)
移除	   | remove(o)   | poll(o)	 | take(o)	 | poll(timeout, timeunit)
检查	   | element(o)	 | peek(o)	 |	         |

四组不同的行为方式解释：
1）抛异常：如果试图的操作无法立即执行，抛一个异常。
2）特定值：如果试图的操作无法立即执行，返回一个特定的值(常常是 true / false)。
3）阻塞：如果试图的操作无法立即执行，该方法调用将会发生阻塞，直到能够执行。
4）超时：如果试图的操作无法立即执行，该方法调用将会发生阻塞，直到能够执行，但等待时间不会超过给定值。返回一个特定值以告知该操作是否成功(典型的是 true / false)。

BlockingQueue 的实现
BlockingQueue 是个接口，你需要使用它的实现之一来使用 BlockingQueue。java.util.concurrent 具有以下 BlockingQueue 接口的实现(Java 6)：
1）ArrayBlockingQueue
2）DelayQueue
3）LinkedBlockingQueue
4）PriorityBlockingQueue
5）SynchronousQueue

* 阻塞双端队列 BlockingDeque

在线程既是一个队列的生产者又是这个队列的消费者的时候可以使用到 BlockingDeque。如果生产者线程需要在队列的两端都可以插入数据，消费者线程需要在队列的两端都可以移除数据，这个时候也可以使用 BlockingDeque
![](/image/java-concurrency-2.png)

操作   | 抛异常	        | 特定值	        | 阻塞	        | 超时
------|-----------------|---------------|---------------|--------------------------------
插入	  | addLast(o)	    | offerLast(o)	| putLast(o)	| offerLast(o, timeout, timeunit)
移除	  | removeLast(o)	| pollLast(o)	| takeLast(o)	| pollLast(timeout, timeunit)
检查	  | getLast(o)	    | peekLast(o)	|               |

java.util.concurrent 包提供了以下 BlockingDeque 接口的实现类：
1) LinkedBlockingDeque

[参考链接English](http://tutorials.jenkov.com/java-util-concurrent/index.html)
[参考链接Chinese](http://blog.csdn.net/defonds/article/details/44021605)

