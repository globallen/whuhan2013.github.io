---
layout: post
title: 校招准备之JAVA基础
date: 2016-7-28
categories: blog
tags: [求职]
description: 
---

**1、equals与==的区别**             
1. == 是一个运算符。                               
2.Equals则是string对象的方法，可以.（点）出来。                   
 
我们比较无非就是这两种 1、基本数据类型比较 2、引用对象比较        
1、基本数据类型比较                   
==和Equals都比较两个值是否相等。相等为true 否则为false；            

2、引用对象比较          
==和Equals都是比较栈内存中的地址是否相等 。相等为true 否则为false；       

需注意几点：             
1、string是一个特殊的引用类型。对于两个字符串的比较，不管是 == 和 Equals 这两者比较的都是字符串是否相同；   
2、当你创建两个string对象时，内存中的地址是不相同的，你可以赋相同的值.      
所以字符串的内容相同。引用地址不一定相同，（相同内容的对象地址不一定相同），但反过来却是肯定的；    
3、基本数据类型比较(string 除外) == 和 Equals 两者都是比较值；         

**2、Object有哪些公用方法**        
clone、equals、hashCode、getClass、wait、notify、notifyAll、toString          
参考：[Object类有哪些公用方法？ - donghaol的专栏 - 博客频道 - CSDN.NET](http://blog.csdn.net/donghaol/article/details/49252383)                 

**3、Java的四种引用，强弱软虚，用到的场景**       

- 强引用       
强引用不会被GC回收，并且在java.lang.ref里也没有实际的对应类型，平时工作接触的最多的就是强引用。        
- 软引用        
如果一个对象只具有软引用，那就类似于可有可物的生活用品。如果内存空间足够，垃圾回收器就不会回收它，如果内存空间不足了，就会回收这些对象的内存。只要垃圾回收器没有回收它，该对象就可以被程序使用。软引用可用来实现内存敏感的高速缓存。

- 弱引用（WeakReference）             
如果一个对象只具有弱引用，那就类似于可有可物的生活用品。弱引用与软引用的区别在于：只具有弱引用的对象拥有更短暂的生命周期。在垃圾回收器线程扫描它 所管辖的内存区域的过程中，一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存。不过，由于垃圾回收器是一个优先级很低的线程， 因此不一定会很快发现那些只具有弱引用的对象。

- 虚引用（PhantomReference）        
"虚引用"顾名思义，就是形同虚设，与其他几种引用都不同，虚引用并不会决定对象的生命周期。如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收。                 
虚引用主要用来跟踪对象被垃圾回收的活动。虚引用与软引用和弱引用的一个区别在于：虚引用必须和引用队列（ReferenceQueue）联合使用。当垃 圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在回收对象的内存之前，把这个虚引用加入到与之关联的引用队列中。程序可以通过判断引用队列中是 否已经加入了虚引用，来了解被引用的对象是否将要被垃圾回收。程序如果发现某个虚引用已经被加入到引用队列，那么就可以在所引用的对象的内存被回收之前采取必要的行动。            
参考：[Java的四种引用，强弱软虚，用到的场景 - To-已陌 - 博客园](http://www.cnblogs.com/yumo/p/4908416.html)       



**4、Hashcode的作用**           
总的来说，Java中的集合（Collection）有两类，一类是List，再有一类是Set。 
你知道它们的区别吗？前者集合内的元素是有序的，元素可以重复；后者元素无序，但元素不可重复。 
那么这里就有一个比较严重的问题了：要想保证元素不重复，可两个元素是否重复应该依据什么来判断呢？ 
这就是Object.equals方法了。但是，如果每增加一个元素就检查一次，那么当元素很多时，后添加到集合中的元素比较的次数就非常多了。 
也就是说，如果集合中现在已经有1000个元素，那么第1001个元素加入集合时，它就要调用1000次equals方法。这显然会大大降低效率。           
于是，Java采用了哈希表的原理。哈希（Hash）实际上是个人名，由于他提出一哈希算法的概念，所以就以他的名字命名了。 


**hashcode方法与equal方法的区别**      

1、默认情况（没有覆盖equals方法）下equals方法都是调用Object类的equals方法，而Object的equals方法主要用于判断对象的内存地址引用是不是同一个地址（是不是同一个对象）。

2 、要是类中覆盖了equals方法，那么就要根据具体的代码来确定equals方法的作用了，覆盖后一般都是通过对象的内容是否相等来判断对象是否相等。

hashCode是根类Obeject中的方法。
 
默认情况下，Object中的hashCode() 返回对象的32位jvm内存地址。也就是说如果对象不重写该方法，则返回相应对象的32为JVM内存地址。


[java中的==、equals和hashCode以及hashCode生成_百度经验](http://jingyan.baidu.com/article/ff41162582507512e5823763.html)


参考：[关于hashCode方法的作用 - huxin1的专栏 - 博客频道 - CSDN.NET](http://blog.csdn.net/huxin1/article/details/6325061)                    

**5、ArrayList、LinkedList、Vector的区别**       
LinkedList 是一个双向链表，线程不安全        

ArrayList 是基于数组实现的List，线程不安全

Vector 多数方法都被synchronized修饰的List实现，线程安全；一般在遗留代码中被使用，现在不推荐。        


**6、String、StringBuffer与StringBuilder的区别**          

- 可变与不可变     
String不可变，其他两个都可变          
- 是否多线程安全         
String中的对象是不可变的，也就可以理解为常量，显然线程安全。                        
StringBuffer对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的                              
StringBuilder并没有对方法进行加同步锁，所以是非线程安全的。              
- StringBuilder与StringBuffer共同点         
同一个父类，但一个线程安全，一个线程不安全，最后，如果程序不是多线程的，那么使用StringBuilder效率高于StringBuffer。

是否可以继承String类                 
String类是final类故不可以继承，一切由final修饰过的都不能继承

**7、Map、Set、List、Queue、Stack的特点与用法**       
Set集合类似于一个罐子，"丢进"Set集合里的多个对象之间没有明显的顺序。        
List集合代表元素有序、可重复的集合，集合中每个元素都有其对应的顺序索引。         
Stack是Vector提供的一个子类，用于模拟"栈"这种数据结构(LIFO后进先出)         
Queue用于模拟"队列"这种数据结构(先进先出 FIFO)。                             
Map用于保存具有"映射关系"的数据，因此Map集合里保存着两组值               

**8、HashMap和HashTable的区别，TreeMap、HashMap、LindedHashMap的区别**           
Hashtable是基于陈旧的Dictionary类的，HashMap是Map接口的一个实现        
Hashtable的方法是线程同步的，而HashMap的方法不是。         
只有HashMap可以让你将空值作为一个表的条目的key或value            


Hashmap 是一个最常用的Map,它根据键的HashCode 值存储数据,根据键可以直接获取它的值，具有很快的访问速度，遍历时，取得数据的顺序是完全随机的。          
LinkedHashMap保存了记录的插入顺序，在用Iterator遍历LinkedHashMap时，先得到的记录肯定是先插入的.也可以在构造时用带参数，按照应用次数排序                                                   
TreeMap取出来的是排序后的键值对。但如果您要按自然顺序或自定义顺序遍历键，那么TreeMap会更好。         

**9、Collection包结构，与Collections的区别**          
Collection是集类，包含List有序列表，Set无序集合以及Map双列集合                 
Collection是集合类的上级接口，子接口主要有Set 和List、Map。             
Collections是针对集合类的一个帮助类，提供了操作集合的工具方法：一系列静态方法实现对各种集合的搜索、排序、线程安全化等操作。               

**10、Excption与Error包结构。OOM你遇到过哪些情况，SOF你遇到过哪些情况。**            
java.lang.OutOfMemoryError: Java heap space ------>java堆内存溢出，此种情况最常见，一般由于内存泄露或者堆的大小设置不当引起。

java.lang.OutOfMemoryError: PermGen space ------>java永久代溢出，即方法区溢出了，一般出现于大量Class或者jsp页面，或者采用cglib等反射机制的情况，因为上述情况会产生大量的Class信息存储于方法区。        

java.lang.StackOverflowError ------> 不会抛OOM error，但也是比较常见的Java内存溢出。JAVA虚拟机栈溢出，一般是由于程序中存在死循环或者深度递归调用造成的，栈大小设置太小也会出现此种溢出。可以通过虚拟机参数-Xss来设置栈的大小。


**11、Java面向对象的三个特征与含义。**         
封装性：它是将类的一些敏感信息隐藏在类的类部，不让外界直接访问到           
继承性：子类通过一种方式来接受父类所有的公有的，受保护的成员变量和成员方法                          
多态性：程序在运行的过程中，同一种类型在不同的条件下表现不同的结果             

**12、Interface与abstract类的区别**           
接口可以多重继承，抽象类不可以          
接口定义方法，不给实现；而抽象类可以实现部分方法         
接口中基本数据类型的数据成员，都默认为static和final，抽象类则不是            

**13、Static class 与non static class的区别**　　　　　　　　　　　
内部静态类不需要有指向外部类的引用。但非静态内部类需要持有对外部类的引用。　　　　　　　　　　
非静态内部类能够访问外部类的静态和非静态成员。静态类不能访问外部类的非静态成员。他只能访问外部类的静态成员。一个非静态内部类不能脱离外部类实体被创建，一个非静态内部类可以访问外部类的数据和方法，因为他就在外部类里面。　　　　　　　　

**14、线程同步的方法：sychronized、lock、reentrantLock等**       
sychronized是java中最基本同步互斥的手段,可以修饰代码块,方法,类，在修饰代码块的时候需要一个reference对象作为锁的对象，在修饰方法的时候默认是当前对象作为锁的对象，在修饰类时候默认是当前类的Class对象作为锁的对象.           
ReentrantLock除了synchronized的功能,多了三个高级功能.                           
等待可中断,在持有锁的线程长时间不释放锁的时候,等待的线程可以选择放弃等待                  

公平锁, 按照申请锁的顺序来一次获得锁称为公平锁.synchronized的是非公平锁,ReentrantLock可以通过构造函数实现公平锁
绑定多个Condition. 通过多次newCondition可以获得多个Condition对象,可以简单的实现比较复杂的线程同步的功能.通过await(),signal();      


**区别**

使用synchronized代码块，可以只对需要同步的代码进行同步，这样可以大大的提高效率。
 
 
使用synchronized 代码块相比方法有两点优势：
 
1、可以只对需要同步的使用
 
2、与wait()/notify()/nitifyAll()一起使用时，比较方便

 
 
wait() 与notify()/notifyAll()
 
这三个方法都是Object的方法，并不是线程的方法！
 
wait():释放占有的对象锁，线程进入等待池，释放cpu,而其他正在等待的线程即可抢占此锁，获得锁的线程即可运行程序。而sleep()不同的是，线程调用此方法后，会休眠一段时间，休眠期间，会暂时释放cpu，但并不释放对象锁。也就是说，在休眠期间，其他线程依然无法进入此代码内部。休眠结束，线程重新获得cpu,执行代码。wait()和sleep()最大的不同在于wait()会释放对象锁，而sleep()不会！


1.Lock能完成几乎所有synchronized的功能，并有一些后者不具备的功能，如锁投票、定时锁等候、可中断锁等候等

2.synchronized 是Java 语言层面的，是内置的关键字；Lock 则是JDK 5中出现的一个包，在使用时，synchronized 同步的代码块可以由JVM自动释放；Lock 需要程序员在finally块中手工释放，如果不释放，可能会引起难以预料的后果（在多线程环境中）。


[对比synchronized与java.util.concurrent.locks.Lock 的异同 - 敬诚为之 - 博客频道 - CSDN.NET](http://blog.csdn.net/hintcnuie/article/details/11022049)

**15、锁的等级：方法锁、对象锁、类锁**　　　　　
　　　　　　　
方法锁，synchronized标记的方法　　　　　　　　　
对象锁，在方法上加了synchronized的锁，或者synchronized(this）的代码段　　　　　　　　　
类锁，在代码中的方法上加了static和synchronized的锁，因为在静态方法中加同步锁会锁住整个类　　　　

**16、写出生产者消费者模式**           
在实际的软件开发过程中，经常会碰到如下场景：某个模块负责产生数据，这些数据由另一个模块来负责处理（此处的模块是广义的，可以是类、函数、线程、进程等）。产生数据的模块，就形象地称为生产者；而处理数据的模块，就称为消费者。 
　 
单单抽象出生产者和消费者，还够不上是生产者／消费者模式。该模式还需要有一个缓冲区处于生产者和消费者之间，作为一个中介。生产者把数据放入缓冲区，而消费者从缓冲区取出数据               

阻塞队列实现生产者消费者模式超级简单，它提供开箱即用支持阻塞的方法put()和take()，开发者不需要写困惑的wait-nofity代码去实现通信。BlockingQueue 一个接口，Java5提供了不同的现实，如ArrayBlockingQueue和LinkedBlockingQueue，两者都是先进先出（FIFO）顺序。而ArrayLinkedQueue是自然有界的，LinkedBlockingQueue可选的边界。
　　　　

**17、ThreadLocal的设计理念与作用**          
ThreadLocal并不是一个Thread，而是Thread的局部变量，也许把它命名为ThreadLocalVariable更容易让人理解一些         
当使用ThreadLocal维护变量时，ThreadLocal为每个使用该变量的线程提供独立的变量副本，所以每一个线程都可以独立地改变自己的副本，而不会影响其它线程所对应的副本。          

**18、ThreadPool用法与优势**         
线程池是为突然大量爆发的线程设计的，通过有限的几个固定线程为大量的操作服务，减少了创建和销毁线程所需的时间，从而提高效率。           

- FixedThreadPool(int nThreads):         创建一个可重用的固定线程数的线程池，如果池中所有的nThreads个线程都处于活动状态时提交任务(任务通常是Runnable或Callable对象), 任务将在队列中等待, 直到池中出现可用线程
- CachedThreadPool()         
调用此方法创建的线程池可根据需要自动调整池中线程的数量，执行任务时将重用存在先前创建的线程(如果池中存在可用线程的话). 如果池中没有可用线程, 将创建一个新的线程, 并将其添加到池中. 池中的线程超过60秒未被使用就会被销毁, 因此长时间保持空闲的
- SingleThreadExecutor()          
创建一个单线程的Executor. 这个Executor保证按照任务提交的顺序依次执行任务.
- ScheduledThreadPool(int corePoolSize)        
创建一个可重用的固定线程数的线程池.       

ScheduledExecutorService是ExecutorService的子接口, 调用ScheduledExecutorService的相关方法, 可以延迟或定期执行任务.
以上静态方法均使用默认的ThreadFactory(即Executors.defaultThreadFactory()方法的返回值)创建线程, 如果想要指定ThreadFactory, 可调用他们的重载方法.通过指定ThreadFactory, 可以定制新建线程的名称, 线程组, 优先级, 守护线程状态等.                                
如果Executors提供的创建线程池的方法无法满足要求, 可以使用ThreadPoolExecutor类创建线程池.          

**19、Concurrent包里的其他东西：ArrayBlockingQueue、CountDownLatch等等。**           
“一个同步辅助类，在完成一组正在其他线程中执行的操作之前，它允许一个或多个线程一直等待。 用给定的计数 初始化 CountDownLatch。由于调用了 countDown() 方法，所以在当前计数到达零之前，await 方法会一直受阻塞。之后，会释放所有等待的线程，await 的所有后续调用都将立即返回。这种现象只出现一次——计数无法被重置。如果需要重置计数，请考虑使用 CyclicBarrier。” 

这就是说，CountDownLatch可以用来管理一组相关的线程执行，只需在主线程中调用CountDownLatch 的await方法（一直阻塞），让各个线程调用countDown方法。当所有的线程都只需完countDown了，await也顺利返回，不再阻塞了。在这样情况下尤其适用：将一个任务分成若干线程执行，等到所有线程执行完，再进行汇总处理。 



**20、wait()和sleep()的区别。**          
sleep指线程被调用时，占着CPU不工作，形象地说明为“占着CPU睡觉”，此时，系统的CPU部分资源被占用，其他线程无法进入，会增加时间限制。                   
wait指线程处于进入等待状态，形象地说明为“等待使用CPU”，此时线程不占用任何资源，不增加时间限制         

**21、Java IO与NIO。**          
Java NIO和IO之间第一个最大的区别是，IO是面向流的，NIO是面向缓冲区的。 Java IO面向流意味着每次从流中读一个或多个字节，直至读取所有字节，它们没有被缓存在任何地方。此外，它不能前后移动流中的数据。如果需要前后移动从流中读取的数据，需要先将它缓存到一个缓冲区。 Java NIO的缓冲导向方法略有不同。数据读取到一个它稍后处理的缓冲区，需要时可在缓冲区中前后移动。这就增加了处理过程中的灵活性。但是，还需要检查是否该缓冲区中包含所有您需要处理的数据。而且，需确保当更多的数据读入缓冲区时，不要覆盖缓冲区里尚未处理的数据。


**22、反射的作用原理**            
JAVA反射（放射）机制：Reflection，Java程序可以加载一个运行时才得知名称的class，获悉其完整构造（但不包括methods定义），并生成其对象实体、或对其fields设值、或唤起其methods。                 

用途：Java反射机制主要提供了以下功能： 在运行时判断任意一个对象所属的类；在运行时构造任意一个类的对象；在运行时判断任意一个类所具有的成员变量和方法；在运行时调用任意一个对象的方法；生成动态代理。              

**23、泛型常用特点，List<String>能否转为List<Object>**          

泛型，即“参数化类型”。一提到参数，最熟悉的就是定义方法时有形参，然后调用此方法时传递实参。那么参数化类型怎么理解呢？顾名思义，就是将类型由原来的具体的类型参数化，类似于方法中的变量参数，此时类型也定义成参数形式（可以称之为类型形参），然后在使用/调用时传入具体的类型（类型实参）
在Java的泛型接口中 把List < String > 转换成List < Object > 是可以的。


**24、JNI的使用**           
JNI作为java和操作系统间的一个直接接口，可以通过JNI使得java直接调用操作系统的资源。目前JNI只能通过c/C++实现，因为jni只是对操作系统资源调用的一个桥接过程。所以理论上在windows下只要是dll文件均可以被调用。            

jni一般有以下一些应用场景                                                                                 
1.高性能 ，在一些情况下因为处理运算量非常大，为了获取高性能，直接使用java是不能胜任的，如：一些图形的处理   
2.调用一些硬件的驱动或者一些软件的驱动，比如调用一些外部系统接口的驱动，如：读卡器的驱动，OCI驱动          
3.需要使用大内存，远远超过jvm所能分配的内存，如：进程内Cache            
4.调用C或者操作系统提供的服务，如：java调用搜索服务，其中搜索是由C/C++实现的，不过这个一般可以设计成更加通用的方式，比如soa的方式                                                                                 
所有这些场景的前提是牺牲了java代码的可移植性，不同的os，甚至版本都需要写不同版本的native实现            



**25、九种基本数据类型与大小**        

int-char-long各占多少字节数

byte 位数 8 字节数 1

short 16 2

int 32 4

long 64 8

float 32 4

double 64 8

char 16 2


**26、Java 注解**

注解是 Java 5 的一个新特性。注解是插入你代码中的一种注释或者说是一种元数据（meta data）。这些注解信息可以在编译期使用预编译工具进行处理（pre-compiler tools），也可以在运行期使用 Java 反射机制进行处理。

能够添加到 Java 源代码的语法元数据。类、方法、变量、参数、包都可以被注解，可用来将信息元数据与程序元素进行关联。Annotation 中文常译为“注解”


**作用**

a. 标记，用于告诉编译器一些信息               
b. 编译时动态处理，如动态生成代码                 
c. 运行时动态处理，如得到注解信息           

这里的三个作用实际对应着后面自定义 Annotation 时说的 @Retention 三种值分别表示的 Annotation


**Annotation 分类**

1 标准 Annotation                       
包括 Override, Deprecated, SuppressWarnings，标准 Annotation 是指 Java 自带的几个 Annotation，上面三个分别表示重写函数，不鼓励使用(有更好方式、使用有风险或已不在维护)，忽略某项 Warning

2 元 Annotation                    
@Retention, @Target, @Inherited, @Documented，元 Annotation 是指用来定义 Annotation 的 Annotation，在后面 Annotation 自定义部分会详细介绍含义

3 自定义 Annotation                 
自定义 Annotation 表示自己根据需要定义的 Annotation，定义时需要用到上面的元 Annotation
这里只是一种分类而已，也可以根据作用域分为源码时、编译时、运行时 Annotation，后面在自定义 Annotation 时会具体介绍

### 多线程常见面试题

**27、Java 中如何停止一个线程？**

Java 提供了很丰富的 API 但没有为停止线程提供 API。JDK 1.0 本来有一些像 stop ()， suspend () 和 resume ()的控制方法但是由于潜在的死锁威胁因此在后续的 JDK 版本中他们被弃用了，之后 Java API 的设计者就没有提供一个兼容且线程安全的方法来停止一个线程。当 run () 或者 call () 方法执行完的时候线程会自动结束，如果要手动结束一个线程，你可以用 volatile 布尔变量来退出 run ()方法的循环或者是取消任务来中断线程

参考：[50个Java多线程面试题_笔试面经_牛客网](http://www.nowcoder.com/discuss/334?type=0&order=0&pos=30&page=1)

**什么是 FutureTask？**

在 Java 并发程序中 FutureTask 表示一个可以取消的异步运算。它有启动和取消运算、查询运算是否完成和取回运算结果等方法。只有当运算完成的时候结果才能取回，如果运算尚未完成 get 方法将会阻塞。一个 FutureTask 对象可以对调用了 Callable 和 Runnable 的对象进行包装，由于 FutureTask 也是调用了 Runnable 接口所以它可以提交给 Executor 来执行。


**Java 中堆和栈有什么不同？**

为什么把这个问题归类在多线程和并发面试题里？因为栈是一块和线程紧密相关的内存区域。每个线程都有自己的栈内存，用于存储本地变量，方法参数和栈调用，一个线程中存储的变量对其它线程是不可见的。而堆是所有线程共享的一片公用内存区域。对象都在堆里创建，为了提升效率线程会从堆中弄一个缓存到自己的栈，如果多个线程使用该变量就可能引发问题，这时 volatile 变量就可以发挥作用了，它要求线程从主存中读取变量的值。

**什么是线程池？ 为什么要使用它？**

创建线程要花费昂贵的资源和时间，如果任务来了才创建线程那么响应时间会变长，而且一个进程能创建的线程数有限。为了避免这些问题，在程序启动的时候就创建若干线程来响应处理，它们被称为线程池，里面的线程叫工作线程。从 JDK1.5 开始，Java API 提供了 Executor 框架让你可以创建不同的线程池。比如单线程池，每次处理一个任务；数目固定的线程池或者是缓存线程池（一个适合很多生存期短的任务的程序的可扩展线程池）


**有三个线程 T1，T2，T3，怎么确保它们按顺序执行？**

在多线程中有多种方法让线程按特定顺序执行，你可以用线程类的 join ()方法在一个线程中启动另一个线程，另外一个线程完成该线程继续执行。为了确保三个线程的顺序你应该先启动最后一个(T3 调用 T2，T2 调用 T1)，这样 T1 就会先完成而 T3 最后完成。

**java之yield(),sleep(),wait()区别详解-备忘笔记**     

Thread.yield()方法作用是：暂停当前正在执行的线程对象，并执行其他线程。



### 下面的来自Java研发方向如何准备BAT技术面试答案

[Java研发方向如何准备BAT技术面试答案（上）_资源分享_牛客网](http://www.nowcoder.com/discuss/6890?type=0&order=0&pos=9&page=1)

**28、面向对象和面向过程的区别**    

- 面向过程               
优点：性能比面向对象高，因为类调用时需要实例化，开销比较大，比较消耗资源;比如单片机、嵌入式开发、Linux/Unix等一般采用面向过程开发，性能是最重要的因素。             
缺点：没有面向对象易维护、易复用、易扩展

- 面向对象                            
优点：易维护、易复用、易扩展，由于面向对象有封装、继承、多态性的特性，可以设计出低耦合的系统，使系统更加灵活、更加易于维护               
缺点：性能比面向过程低          


**29、hashCode和equals方法的关系**　　　　　　　　　　　　　　　
equals相等，hashcode必相等；hashcode相等，equals可能不相等。


**30、自动装箱与拆箱**     
装箱：将基本类型用它们对应的引用类型包装起来；      
拆箱：将包装类型转换为基本数据类型；               
Java使用自动装箱和拆箱机制，节省了常用数值的内存开销和创建对象的开销，提高了效率，由编译器来完成，编译器会在编译期根据语法决定是否进行装箱和拆箱动作。


**31、多线程如何进行信息交互**                 
void notify() 唤醒在此对象监视器上等待的单个线程。             
void notifyAll() 唤醒在此对象监视器上等待的所有线程。                                       
void wait() 导致当前的线程等待，直到其他线程调用此对象的notify()方法或notifyAll()方法。       
void wait(long timeout) 导致当前的线程等待，直到其他线程调用此对象的notify()方法或notifyAll()方法，或者超过指定的时间量。                                    
void wait(long timeout, int nanos) 导致当前的线程等待，直到其他线程调用此对象的notify()方法或notifyAll()方法，或者其他某个线程中断当前线程，或者已超过某个实际时间量。            


**32、volatile关键字**         

用volatile修饰的变量，线程在每次使用变量的时候，都会读取变量修改后的最的值。volatile很容易被误用，用来进行原子性操作。

锁提供了两种主要特性：互斥（mutual exclusion）和可见性（visibility）。互斥即一次只允许一个线程持有某个特定的锁，因此可使用该特性实现对共享数据的协调访问协议，这样，一次就只有一个线程能够使用该共享数据。可见性必须确保释放锁之前对共享数据做出的更改对于随后获得该锁的另一个线程是可见的，如果没有同步机制提供的这种可见性保证，线程看到的共享变量可能是修改前的值或不一致的值，这将引发许多严重问题。Volatile变量具有synchronized的可见性特性，但是不具备原子特性。这就是说线程能够自动发现 volatile 变量的最新值。


**33、序列化与反序列化**      

把对象转换为字节序列的过程称为对象的序列化。          
把字节序列恢复为对象的过程称为对象的反序列化。         

对象的序列化主要有两种用途：               
一.把对象的字节序列永久地保存到硬盘上，通常存放在一个文件中；               
二.在网络上传送对象的字节序列。           

当两个进程在进行远程通信时，彼此可以发送各种类型的数据。无论是何种类型的数据，都会以二进制序列的形式在网络上传送。发送方需要把这个Java对象转换为字节序列，才能在网络上传送；接收方则需要把字节序列再恢复为Java对象。

常见的序列化协议有哪些：
Protobuf, Thrift, Hessian, Kryo   

**34、xml解析方式**            
一.DOM(JAXP Crimson解析器)             
二.SAX             
三.JDOM          
四.DOM4J       

区别：               
一.DOM4J性能最好，连Sun的JAXM也在用DOM4J。目前许多开源项目中大量采用DOM4J，例如大名鼎鼎的hibernate也用DOM4J来读取XML配置文件。如果不考虑可移植性，那就采用DOM4J.

二.JDOM和DOM在性能测试时表现不佳，在测试10M文档时内存溢出。在小文档情况下还值得考虑使用DOM和JDOM。虽然JDOM的开发者已经说明他们期望在正式发行版前专注性能问题，但是从性能观点来看，它确实没有值得推荐之处。另外，DOM仍是一个非常好的选择。DOM实现广泛应用于多种编程语言。它还是许多其它与XML相关的标准的基础，因为它正式获得W3C推荐(与基于非标准的Java模型相对)，所以在某些类型的项目中可能也需要它(如在JavaScript中使用DOM)。    

三.SAX表现较好，这要依赖于它特定的解析方式－事件驱动。一个SAX检测即将到来的XML流，但并没有载入到内存(当然当XML流被读入时，会有部分文档暂时隐藏在内存中)。

两种解析思想：

- dom解析：       
将整个XML使用类似树的结构保存在内存中，便用增删改查，但占用内存          
- sax解析：          
逐行解析，当遇到标签时触发解析处理，采用事件处理的方式解析XML，只能用于读取，无法用于增删改。


