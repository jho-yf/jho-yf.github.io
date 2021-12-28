# JUC并发编程

[toc]

## JUC概述

### JUC简介

JUC就是`java.util.concurrent`工具包的简称，这是一个**处理线程**的工具包，JDK1.5开始出现。

### 线程和进程概念

**进程（Process）**是计算机中的程序关于某数据集合上的一次运行活动，是系统进行资源分配和调度的基本单位，是操作系统结构的基础。

**线程（Thread）**是操作系统能够进行运算调度的最小单位，它被包含在进程之中，是进程中的实际运作单位。

#### 进程和线程

一条**线程**指的是**进程**中一个单一顺序的控制流，一个**进程**中可以并发多个线程，每条**线程**并发执行不同的任务。

> 总的来说：
>
> - 进程：指在系统中正在运行的一个应用程序，程序一旦运行就是进程。
>   - 进程——**系统资源分配的最小单位**
> - 线程：系统分配处理器时间资源的基本单元，或者说进程之后独立执行一个单元执行流。
>   - 线程——**程序执行的最小单位**

### 线程的状态

#### 线程枚举类

```java
// java.lang.Thread.State
public enum State {
        /**
         * Thread state for a thread which has not yet started.
         */
        NEW,		// 新建

        /**
         * Thread state for a runnable thread.  A thread in the runnable
         * state is executing in the Java virtual machine but it may
         * be waiting for other resources from the operating system
         * such as processor.
         */
        RUNNABLE,	// 准备就绪

        /**
         * Thread state for a thread blocked waiting for a monitor lock.
         * A thread in the blocked state is waiting for a monitor lock
         * to enter a synchronized block/method or
         * reenter a synchronized block/method after calling
         * {@link Object#wait() Object.wait}.
         */
        BLOCKED,	// 阻塞

        /**
         * Thread state for a waiting thread.
         * A thread is in the waiting state due to calling one of the
         * following methods:
         * <ul>
         *   <li>{@link Object#wait() Object.wait} with no timeout</li>
         *   <li>{@link #join() Thread.join} with no timeout</li>
         *   <li>{@link LockSupport#park() LockSupport.park}</li>
         * </ul>
         *
         * <p>A thread in the waiting state is waiting for another thread to
         * perform a particular action.
         *
         * For example, a thread that has called <tt>Object.wait()</tt>
         * on an object is waiting for another thread to call
         * <tt>Object.notify()</tt> or <tt>Object.notifyAll()</tt> on
         * that object. A thread that has called <tt>Thread.join()</tt>
         * is waiting for a specified thread to terminate.
         */
        WAITING,	// 等待（不见不散）

        /**
         * Thread state for a waiting thread with a specified waiting time.
         * A thread is in the timed waiting state due to calling one of
         * the following methods with a specified positive waiting time:
         * <ul>
         *   <li>{@link #sleep Thread.sleep}</li>
         *   <li>{@link Object#wait(long) Object.wait} with timeout</li>
         *   <li>{@link #join(long) Thread.join} with timeout</li>
         *   <li>{@link LockSupport#parkNanos LockSupport.parkNanos}</li>
         *   <li>{@link LockSupport#parkUntil LockSupport.parkUntil}</li>
         * </ul>
         */
        TIMED_WAITING,		// 计时等待（过时不候）

        /**
         * Thread state for a terminated thread.
         * The thread has completed execution.
         */
        TERMINATED;			// 终结
    }
```

#### wait/sleep的区别

1. `sleep`是`Thread`的静态方法，`wait`是`Object`的方法，任何对象实例都能调用
2. `sleep`不会释放锁，它也不需要占用锁；`wait`会释放锁，但是调用它的前提是当前线程占有锁（即代码要在`synchronized`中）
3. 它们都可以被`interrupted`方法中断

### 并发和并行

> 对于单核CPU来说，同一时刻只能运行一个线程，只是CPU在线程之间切换执行指令速度很快，所以可以理解成为单核CPU可以同时运行多个线程。

**串行模式**：**串行**表示所有任务都一一按照先后顺序进行。

**并行模式**：**并行**意味着可以同时取得多个任务，并同时去执行所取得的这些任务。

**并发：并发（concurrent）指的是多个程序可以同时运行的现象，更细化的是多进程可以同时运行或者多指令可以同时运行**。

> 关于并行和并发的区别：[面试必考的：并发和并行有什么区别？](https://cloud.tencent.com/developer/article/1424249)
>
> 总的来说：
>
> - **并发**：同一时刻多个线程在访问同一个资源，**n个线程 vs 1个资源**
> - **并行**：多项工作同时执行后汇总
>
> 举例：
>
> ​	公司领导要求A、B、C三人完成一份需求文档，A负责写“背景”，C负责写“需求分析”，D负责写”系统设计“。
>
> ​	**并发**：公司里只有一台电脑，此时A先占用电脑写，写了一部分之后A去上厕所；B赶紧占用电脑继续写，写了一部分之后，B被领导叫走；C赶紧占用电脑继续写...
>
> ​	**并行**：A、B、C三人分别在各自电脑完成后，进行汇总。

### 管程

**管程 (英语：Monitors，也称为监视器)** 是一种程序结构，结构内的多个子程序（[对象](https://zh.wikipedia.org/wiki/对象_(计算机科学))或[模块](https://zh.wikipedia.org/wiki/模組_(程式設計))）形成的多个[工作线程](https://zh.wikipedia.org/wiki/工作_(資訊科學))互斥访问共享资源。（来自[维基百科：管程](https://zh.wikipedia.org/wiki/%E7%9B%A3%E8%A6%96%E5%99%A8_(%E7%A8%8B%E5%BA%8F%E5%90%8C%E6%AD%A5%E5%8C%96))）

> 管程是一种同步机制，保证同一时间，只有一个线程访问被保护数据或者代码，JVM同步是使用**管程**对象实现的

### 用户线程和守护线程

Java 中的线程分为两类，分别为**daemon 线程（守护线程）**和**user 线程（用户线程）**。（来自：[Java守护线程与用户线程的区别](https://juejin.cn/post/7020953597252730910)）

- 守护线程又称Daemon线程，运行在后台，看不见；(垃圾回收线程)
- 用户线程运行在前台，看的见。（自定义线程、main线程）

#### [示例代码](https://github.com/jho-yf/yf-java-learning/blob/main/yf-juc-learning/src/main/java/cn/jho/juc/juc01/DaemonThreadDemo.java)

```java
// 以下示例运行结束发现，主线程（main）结束了，用户线程（myThread）还在运行，JVM存活
public class DaemonThreadDemo {

    public static void main(String[] args) {
        Thread myThread = new Thread(() -> {
            System.out.println(Thread.currentThread().getName() + "::" + Thread.currentThread().isDaemon());
            while (true) {

            }
        }, "myThread");
        myThread.start();

        System.out.println(Thread.currentThread().getName() + "结束...");
    }

}
```

将`myThread`设置成为守护线程

```java
// 以下示例运行结束发现，主线程（main）结束了，用户线程（myThread）也结束了，JVM结束
public class DaemonThreadDemo {

    public static void main(String[] args) {
        Thread myThread = new Thread(() -> {
            System.out.println(Thread.currentThread().getName() + "::" + Thread.currentThread().isDaemon());
            while (true) {

            }
        }, "myThread");

        // 设置守护线程，需要在start之前设置
        myThread.setDaemon(true);

        myThread.start();
        System.out.println(Thread.currentThread().getName() + "结束...");
    }

}
```

> 总结：
>
> - 若存在**任何一个用户线程未结束**，JVM存活。
>
> - 若**只剩下守护线程未结束**，JVM结束。



## Lock接口



## 线程间通信



## 线程间定制化通信



## 集合的线程安全



## 多线程锁



## Callable接口



## JUC强大的辅助类



### CountDownLatch 减少技术



### CyclicBarrier 循环栅栏



### Semaphore 信号灯



## ReetrantReadWriteLock 读写锁



## BlockingQueue 阻塞队列



## ThreadPool 线程池



## Fork/Join 分支合并框架



## CompletableFuture 异步回调



