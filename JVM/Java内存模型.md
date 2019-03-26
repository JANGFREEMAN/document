## Java内存模型

Java程序启动时，JVM会划分一块内存区域。Java 运行时候区域划分，分为：

1.  方法区（Method Area）：存放类结构信息，运行时常量池，该内存是所有线程共享。
2. 堆（head）：创建的对象实例存放在该内存，受GC管理，所有有时候也称为GC堆，该内存也所有线程共享。
3. 程序计数器（PC ）：
4. 虚拟机栈：（VM Stack）：虚拟机栈是线程独有，每一个线程都有自己的虚拟机栈，虚拟机栈处理Java方法的执行，每一个方法被执行时，都会创建一个栈帧，用于存储局部变量表，方法出口等信息。Java方法从开始到执行完成对应一个栈帧的入栈到出栈的过程。
5. 本地方法栈（Native Stack）：线程独有的，功能和虚拟机栈一样。只不过处理的方法是本地方法。

![JVM运行时数据区](http://ww1.sinaimg.cn/large/0067Oxblgy1g05rmcdhdvj30hc0b8af2.jpg)

## 垃圾回收

   		GC回收会根据内存使用情况来做垃圾回收，真是因为这样的机制所以在程序中不需要显示的做回收动作，这也就造成了垃圾回收时间不确定的原因。GC回收主要是堆内存中一些不可用对象实例，由于GC回收消耗资源比较大，所以采用**分代**方式进行回收。Java对堆内存中对象做生命特征分析，将对象分为三个世代。GC对不同世代的对象采用对应的垃圾回收算法。

新生代（Young Generation）：

​	Eden：在程序中创建的对象都会放到该内存中。

​	S0: 存活时间较长，没有被垃圾回收的对象会从Eden搬迁到S0上。

​	S1: 同理，存活时间更长，没有被垃圾回收会从S0搬迁到S1上。

老生代（Old Generation）: 同理，存活时间更上，多次垃圾回收没有被清除，会从S1搬迁过来。

持久代（Permanment Generation）: 用于存放静态文件，如静态类。



## 堆（Head）组成



![堆内存组成图](http://ww1.sinaimg.cn/large/0067Oxblgy1g05retpx93j30hk0bcgoa.jpg)



## GC执行机制

   由于对对象进行分代划分，在垃圾回收区域上和时间上也不同，GC回收采用两种机制：Scavenge GC和Full GC。

###  Scavenge GC

一般对象创建并在Eden区申请空间失败，这种情况会出发Scavenge GC，在对Eden区非存活对象进行回收，把存活对象已入Survivor区。该次回收不影响老生代（Old Generation）。Eden分配的空间一般比较小，发生GC的次数也比较频繁，所以在对Eden区做垃圾回收的算法一般采用效率高的。

### Full GC

Full GC是对堆整个区间进行垃圾回收，Full GC回收的效率比较慢，因此要尽可能减少Full GC的次数。在JVM调优的时候，有很大一部分工作是对Full GC的调优，一般发生Full GC的情况有：

1.  年老代被写满
2. 持久代被写满
3. 显示的调用System.gc()
4. 上一次GC之后，堆各域分配策略动态变化



## 运行时常量池

JDK1-JDK1.6版本运行时常量池存在方法区中，该区是全局共享的。该内存是是动态的，可以在运行期间往常量池中加入常量，典型的应用就是String的Intern方法。

## 类常量池

Class文件存储类的版本号、方法、字段、常量等信息，编译后的Class文件的每个字节存储的内容需要符合规范，这样才能被Jvm执行、加载。在Class文件中，有一个字段是用来存储常量的，故称为类常量池。该常量池存放的是编译期间就确定好的常量。

## 参考文献



1. [深入理解垃圾回收机制](https://www.cnblogs.com/andy-zcx/p/5522836.html)
2. [深入Java内存模型](https://blog.csdn.net/javazejian/article/details/72772461 )
3. [Java内存模型讲解](https://segmentfault.com/a/1190000002579346)
4. [JVM字符串常量池与运行时常量池](https://blog.csdn.net/Sugar_Rainbow/article/details/68150249)