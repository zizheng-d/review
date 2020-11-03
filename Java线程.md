## Q：什么是线程？

线程是计算机中最小的运算单元，他的父级是进程。是进程中实际执行运算的单位。

## Q：Java中线程有哪些状态？

初始、运行中、等待、就绪、阻塞、终止

## Q：Java中实现线程有哪些方法？

1. 继承Thread类，重写run()方法

2. 实现Runable接口，实现run()方法

3. 实现Callable接口，实现call()方法

   其中Runable和Thread执行时是不能返回执行结果或者异常抛出的。只有Callable方法可以抛出异常或者返回结果。Callable返回的结果要使用Future来接收

## Q：Start方法和run方法有什么区别？

Start方法时用来启动线程的，线程启动会进入就绪状态，等到CPU有资源后会调用run方法来执行该方法内的代码。
如果直接调用run方法并不会启动一个新线程。总的来说就是Start方法可以启动线程来达到多线程的目的而run方法只是Thread对象的一个成员方法。直接调用并不会去新建一个线程。

## Q：什么是线程池？为什么会有线程池？

创建线程，销毁线程是一个很大的开销，尤其是当我的线程数过多的时候。这个时候创建和销毁线程占用了绝大部分的资源。所以我们再程序启动的时候就初始化指定的线程数保存起来。当某些任务需要多线程去使用的时候，可以直接把任务提交到线程池当中。让线程池来进行任务的执行。这样就减少了创建、销毁线程的开销。同时也更容易控制执行的线程数量从而控制计算机的性能。

## Q：线程池有哪些参数？

corePoolSize：线程池大小
maximumPoolSize：线程池最大大小
runnableTaskQueue：任务队列
ThreadFactory：用于创建线程的工厂
RejectedExecutionHandler：拒绝策略
keepAliveTime：线程最大生存时间
TimeUnit：线程最大生存时间的时间单位

## Q：Java中线程池有哪几种？

1. CachedThreadPool()：可缓存线程池。
2. FixedThreadPool()：定长线程池
3. ScheduledThreadPool()：定时线程池
4. SingleThreadExecutor()：单线程化的线程池

