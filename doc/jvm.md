## JVM

### 1. 什么情况下会发生栈内存溢出。

> * 如果线程请求的栈深度大于虚拟机所允许的深度，将抛出 **StackOverflowError** 异常。
> * 如果虚拟机在动态扩展栈时无法申请到足够的内存空间，则抛出 **OutOfMemoryError** 异常。
>
> [Java 内存区域与内存溢出](https://wiki.jikexueyuan.com/project/java-vm/storage.html)

### 2. Java内存

> 
>
> ```mermaid
> graph LR
> A[Java内存] --> B[私有内存区]
> A --> C[共享内存区]
> B --> D[虚拟机]
> B --> E[程序计数器]
> B --> F[本地方法栈]
> C --> G[Java堆]
> C --> H[方法区]
> H --> I[非堆内存&#40永久&#41]
> ```
>
> * **私有内存区**：伴随线程的产生而产生，一旦线程终止，私有内存区也会自动消除
> * **程序计数器**：指示当前程序执行到了哪一行，执行Java方法时记录正在执行的虚拟机字节码指令地址；执行本地方法时，计数器值为null
> * **虚拟机栈**：用于执行Java方法，栈帧存储局部变量表，操作数栈，动态链接，方法返回地址和一些额外的符加信息。程序执行时入栈；执行完成后栈帧出栈。
> * **Java堆**：Java虚拟机管理的内存中最大的一块，所有线程共享，几乎所有的对象实例和数组都在这里分配内存。GC主要就是在Java堆中进行的。 堆内存又分为：新生代（新生代又分为Eden80%，Survivor20%）和老年代（Old），并且一般新生代的空间比老年代大。
> * **方法区**：只有一个方法区共享。实际也是堆,只是用于存储类,常量相关的信息，来存放程序中永远不变或唯一的内容(类信息【Class对象】，静态变量，字符串常量等)。但是已经被最新的 JVM 取消了。现在，被加载的类作为元数据加载到底层操作系统的本地内存区。

### 3. JVM 的内存结构，Eden 和 Survivor 比例。

> Eden区是一块，Survivor区是两块。
>
> Eden区和Survivor区的比例是8：1：1
>
> **JVM内存的结构为**
>
> * **堆**：存放对象
> * **栈**：运行时存放栈帧
> * **程序计数器**
> * **方法区**：存放类和常量
>
> 1.8之后取消了永久代，方法区概念上还是存在的，原先永久代中类的元信息会被放入本地内存，将类的静态变量和内部字符串放入到 java 堆中
>
> [新生代Eden与两个Survivor区的解释](https://blog.csdn.net/lojze_ly/article/details/49456255)

### 4. JVM 中一次完整的 GC（垃圾回收） 流程是怎样的，对象如何晋升到老年代，说说你知道的几种主要的 JVM 参数。

> 对象诞生即新生代（eden），在进行minor gc过程中，如果依旧存活，移动到from，变成Survivor，进行标记代数，如此检查一定次数后，晋升为老年代。
>
> [JVM系列二:GC策略&内存申请、对象衰老](https://www.cnblogs.com/redcreen/archive/2011/05/04/2037056.html)
>
> [JVM实用参数系列](http://ifeve.com/useful-jvm-flags/)
>
> [jvm参数详解](https://wangkang007.gitbooks.io/jvm/content/jvmcan_shu_xiang_jie.html)

### 5. 你知道哪几种垃圾收集器，各自的优缺点，重点讲下 cms，包括原理，流程，优缺点。及垃圾回收算法的实现原理。

> Serial（复制算法）、parNew（停止-复制算法）、ParallelScavenge（停止-复制算法）、SerialOld（标记-整理算法）、ParallelOld（停止-复制算法）、CMS（标记-清理算法）、G1
>
> [垃圾回收算法梳理](https://wangkang007.gitbooks.io/jvm/content/chapter1.html)
>
> [扒一扒JVM的垃圾回收机制，下次面试你准备好了吗](https://www.cnblogs.com/1024Community/p/honery.html)

### 6.当出现了内存溢出，你怎么排错。

> 首先分析是什么类型的内存溢出，对应的调整参数或者优化代码。
>
> [jvm调优](https://wangkang007.gitbooks.io/jvm/content/4jvmdiao_you.html)

### 7. JVM 内存模型的相关知识了解多少，比如重排序，内存屏障，happen-before，主内存，工作内存等。

> * 内存屏障：为了保障执行顺序和可见性的一条cpu指令
> * 重排序：为了提高性能，编译器和处理器会对执行进行重拍
> * happen-before：操作间执行的顺序关系。有些操作先发生。
> * 主内存：共享变量存储的区域即是主内存
> * 工作内存：每个线程copy的本地内存，存储了该线程以读/写共享变量的副本
>
> [深入理解Java内存模型（一）——基础](http://ifeve.com/java-memory-model-1/)
> [java内存模型](http://www.jianshu.com/p/d3fda02d4cae)

### 8. 简单说说你了解的类加载器。

> * 类加载器的分类（bootstrap，ext，app，curstom）
> * 类加载的流程（load -> link -> init）
>
> [Java类加载器总结](https://blog.csdn.net/gjanyanlig/article/details/6818655/)

### 9. 讲讲 JAVA 的反射机制。

> Java程序在运行状态可以动态的获取类的所有属性和方法，并实例化该类，调用方法的功能
> [Java基础之—反射（非常重要）](https://blog.csdn.net/sinat_38259539/article/details/71799078)

### 10. 你们线上应用的 JVM 参数有哪些。

> * -server
> * -Xms6000M
> * -Xmx6000M
> * -Xmn500M
> * -XX:PermSize=500M
> * -XX:MaxPermSize=500M
> * -XX:SurvivorRatio=65536
> * -XX:MaxTenuringThreshold=0
> * -Xnoclassgc
> * -XX:+DisableExplicitGC
> * -XX:+UseParNewGC
> * -XX:+UseConcMarkSweepGC
> * -XX:+UseCMSCompactAtFullCollection
> * -XX:CMSFullGCsBeforeCompaction=0
> * -XX:+CMSClassUnloadingEnabled
> * -XX:-CMSParallelRemarkEnabled
> * -XX:CMSInitiatingOccupancyFraction=90
> * -XX:SoftRefLRUPolicyMSPerMB=0
> * -XX:+PrintClassHistogram
> * -XX:+PrintGCDetails
> * -XX:+PrintGCTimeStamps
> * -XX:+PrintHeapAtGC
> * -Xloggc:log/gc.log
>
> ---
>
> **请解释如下 jvm 参数的含义：**
> **-server -Xms512m -Xmx512m -Xss1024K**
> **-XX:PermSize=256m -XX:MaxPermSize=512m -XX:MaxTenuringThreshold=20**
> **-XX:CMSInitiatingOccupancyFraction=80 -XX:+UseCMSInitiatingOccupancyOnly。**
> Server模式启动
> 最小堆内存512m
> 最大512m
> 每个线程栈空间1m
> 永久代256m
> 最大永久代512m
> 最大转为老年代检查次数20
> Cms回收开启时机：内存占用80%
> 命令JVM不基于运行时收集的数据来启动CMS垃圾收集周期

### 11. G1 和 CMS 区别,吞吐量优先和响应优先的垃圾收集器选择。

> * **CMS** 是以获取最短回收停顿时间为目标的收集器。基于标记-清除算法实现。比较占用cpu资源，切易造成碎片。
> * **G1** 是面向服务端的垃圾收集器，是jdk9默认的收集器，基于标记-整理算法实现。可利用多核、多cpu，保留分代，实现可预测停顿，可控。
>
> [浅谈CMS垃圾收集器与G1收集器](http://blog.csdn.net/linhu007/article/details/48897597)

### 12. 32位系统jvm堆内存如下哪一个设置是最大且有效的( B )

> A. -Xmx1000m
> B. -Xmx1500m
> C. -Xmx8G
> D. 无限
>
> JVM最大内存: 首先JVM内存限制于实际的最大物理内存，假设物理内存无限大的话，JVM内存的最大值跟操作系统有很大的关系。简单的说就32位处理器虽然可控内存空间有4GB,但是具体的操作系统会给一个限制，这个限制一般是2GB-3GB（一般来说Windows系统下为1.5G-2G，Linux系统下为2G-3G），而64bit以上的处理器就不会有限制了。

