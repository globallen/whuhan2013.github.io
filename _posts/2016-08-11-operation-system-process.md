---
layout: post
title: 操作系统进程与线程
date: 2016-8-11
categories: blog
tags: [操作系统]
description: 操作系统
---



### 进程的概念和特征      

在多道程序环境下，允许多个程序并发执行，此时它们将失去封闭性，并具有间断性及不可再现性的特征。为此引入了进程(Process)的概念，以便更好地描述和控制程序的并发执行，实现操作系统的并发性和共享性。

为了使参与并发执行的程序（含数据）能独立地运行，必须为之配置一个专门的数据结构，称为进程控制块(Process Control Block, PCB)。系统利用PCB来描述进程的基本情况和运行状态，进而控制和管理进程。相应地，由程序段、相关数据段和PCB三部分构成了进程映像（进程实体）。所谓创建进程，实质上是创建进程映像中的PCB；而撤销进程，实质上是撤销进程的PCB。值得注意的是，进程映像是静态的，进程则是动态的。

注意：PCB是进程存在的唯一标志！

从不同的角度，进程可以有不同的定义，比较典型的定义有：

- 进程是程序的一次执行过程。
- 进程是一个程序及其数据在处理机上顺序执行时所发生的活动。
- 进程是具有独立功能的程序在一个数据集合上运行的过程，它是系统进行资源分配和调度的一个独立单位。

在引入进程实体的概念后，我们可以把传统操作系统中的进程定义为：”进程是进程实体的运行过程，是系统进行资源分配和调度的一个独立单位。“

![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p11.png)

**进程的特征**

进程是由多程序的并发执行而引出的，它和程序是两个截然不同的概念。进程的基本特征是对比单个程序的顺序执行提出的，也是对进程管理提出的基本要求。

- 动态性：进程是程序的一次执行，它有着创建、活动、暂停、终止等过程，具有一定的生命周期，是动态地产生、变化和消亡的。动态性是进程最基本的特征。
- 并发性：指多个进程实体，同存于内存中，能在一段时间内同时运行，并发性是进程的重要特征，同时也是操作系统的重要特征。引入进程的目的就是为了使程序能与其他进程的程序并发执行，以提高资源利用率。
- 独立性：指进程实体是一个能独立运行、独立获得资源和独立接受调度的基本单位。凡未建立PCB的程序都不能作为一个独立的单位参与运行。
- 异步性：由于进程的相互制约，使进程具有执行的间断性，即进程按各自独立的、 不可预知的速度向前推进。异步性会导致执行结果的不可再现性，为此，在操作系统中必须配置相应的进程同步机制。
- 结构性：每个进程都配置一个PCB对其进行描述。从结构上看，进程实体是由程序段、数据段和进程控制段三部分组成的。


### 进程的状态与转换

进程在其生命周期内，由于系统中各进程之间的相互制约关系及系统的运行环境的变化，使得进程的状态也在不断地发生变化（一个进程会经历若干种不同状态）。通常进程有以下五种状态，前三种是进程的基本状态。

![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p12.png)
![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p13.png)
![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p14.png)


### 进程控制
进程控制的主要功能是对系统中的所有进程实施有效的管理，它具有创建新进程、撤销已有进程、实现进程状态转换等功能。在操作系统中，一般把进程控制用的程序段称为原语，原语的特点是执行期间不允许中断，它是一个不可分割的基本单位。

#### 进程的创建

- 给新进程分配一个唯一标识以及进程控制块
- 为进程分配地址空间
- 初始化进程控制块
设置默认值 (如: 状态为 New，...)
- 设置相应的队列指针
如: 把新进程加到就绪队列链表中


#### 进程的撤销       

- 结束进程
收回进程所占有的资源
关闭打开的文件、断开网络连接、回收分配的内存、……
- 撤消该进程的PCB

#### 进程的阻塞    
处于运行状态的进程，在其运行过程中期待某一事件发生，如等待键盘输入、等待磁盘数据传输完成、等待其它进程发送消息，当被等待的事件未发生时，由进程自己执行阻塞原语，使自己由运行态变为阻塞态

### 进程的通信   
进程通信是指进程之间的信息交换。PV操作是低级通信方式，髙级通信方式是指以较高的效率传输大量数据的通信方式。高级通信方法主要有以下三个类。

#### 共享存储

在通信的进程之间存在一块可直接访问的共享空间，通过对这片共享空间进行写/读操作实现进程之间的信息交换。在对共享空间进行写/读操作时，需要使用同步互斥工具（如 P操作、V操作），对共享空间的写/读进行控制。共享存储又分为两种：低级方式的共享是基于数据结构的共享；高级方式则是基于存储区的共享。操作系统只负责为通信进程提供可共享使用的存储空间和同步互斥工具，而数据交换则由用户自己安排读/写指令完成。

需要注意的是，用户进程空间一般都是独立的，要想让两个用户进程共享空间必须通过特殊的系统调用实现，而进程内的线程是自然共享进程空间的。

#### 消息传递

在消息传递系统中，进程间的数据交换是以格式化的消息(Message)为单位的。若通信的进程之间不存在可直接访问的共享空间，则必须利用操作系统提供的消息传递方法实现进程通信。进程通过系统提供的发送消息和接收消息两个原语进行数据交换。

1) 直接通信方式：发送进程直接把消息发送给接收进程，并将它挂在接收进程的消息缓冲队列上，接收进程从消息缓冲队列中取得消息。

2) 间接通信方式：发送进程把消息发送到某个中间实体中，接收进程从中间实体中取得消息。这种中间实体一般称为信箱，这种通信方式又称为信箱通信方式。该通信方式广泛应用于计算机网络中，相应的通信系统称为电子邮件系统。

#### 管道通信

管道通信是消息传递的一种特殊方式。所谓“管道”，是指用于连接一个读进程和一个写进程以实现它们之间通信的一个共享文件，又名pipe文件。向管道（共享文件）提供输入的发送进程（即写进程），以字符流形式将大量的数据送入（写）管道；而接收管道输出的接收进程（即读进程），则从管道中接收（读）数据。为了协调双方的通信，管道机制必须提供以下三方面的协调能力：互斥、同步和确定对方的存在


### 线程的概念和多线程模型

引入进程的目的，是为了使多道程序并发执行，以提高资源利用率和系统吞吐量；而引入线程，则是为了减小程序在并发执行时所付出的时空开销，提高操作系统的并发性能。

线程最直接的理解就是“轻量级进程”，它是一个基本的CPU执行单元，也是程序执行流的最小单元，由线程ID、程序计数器、寄存器集合和堆栈组成。线程是进程中的一个实体，是被系统独立调度和分派的基本单位，线程自己不拥有系统资源，只拥有一点在运行中必不可少的资源，但它可与同属一个进程的其他线程共享进程所拥有的全部资源。一个线程可以创建和撤销另一个线程，同一进程中的多个线程之间可以并发执行。由于线程之间的相互制约，致使线程在运行中呈现出间断性。线程也有就绪、阻塞和运行三种基本状态。

引入线程后，进程的内涵发生了改变，进程只作为除CPU以外系统资源的分配单元，线程则作为处理机的分配单元。

#### 线程与进程的比较

1) 调度。在传统的操作系统中，拥有资源和独立调度的基本单位都是进程。在引入线程的操作系统中，线程是独立调度的基本单位，进程是资源拥有的基本单位。在同一进程中，线程的切换不会引起进程切换。在不同进程中进行线程切换,如从一个进程内的线程切换到另一个进程中的线程时，会引起进程切换。

2) 拥有资源。不论是传统操作系统还是设有线程的操作系统，进程都是拥有资源的基本单位，而线程不拥有系统资源（也有一点必不可少的资源），但线程可以访问其隶属进程的系统资源。

3) 并发性。在引入线程的操作系统中，不仅进程之间可以并发执行，而且多个线程之间也可以并发执行，从而使操作系统具有更好的并发性，提高了系统的吞吐量。

4) 系统开销。由于创建或撤销进程时，系统都要为之分配或回收资源，如内存空间、 I/O设备等，因此操作系统所付出的开销远大于创建或撤销线程时的开销。类似地，在进行进程切换时，涉及当前执行进程CPU环境的保存及新调度到进程CPU环境的设置，而线程切换时只需保存和设置少量寄存器内容，开销很小。此外，由于同一进程内的多个线程共享进程的地址空间，因此，这些线程之间的同步与通信非常容易实现，甚至无需操作系统的干预。

5) 地址空间和其他资源（如打开的文件）：进程的地址空间之间互相独立，同一进程的各线程间共享进程的资源，某进程内的线程对于其他进程不可见。

6) 通信方面：进程间通信(IPC)需要进程同步和互斥手段的辅助，以保证数据的一致性，而线程间可以直接读/写进程数据段（如全局变量）来进行通信。


#### 线程的实现方式
线程的实现可以分为两类：用户级线程(User-LevelThread, ULT)和内核级线程(Kemel-LevelThread,  KLT)。内核级线程又称为内核支持的线程。

在用户级线程中，有关线程管理的所有工作都由应用程序完成，内核意识不到线程的存在。应用程序可以通过使用线程库设计成多线程程序。通常，应用程序从单线程起始，在该线程中开始运行，在其运行的任何时刻，可以通过调用线程库中的派生例程创建一个在相同进程中运行的新线程。图2-2(a)说明了用户级线程的实现方式。

在内核级线程中，线程管理的所有工作由内核完成，应用程序没有进行线程管理的代码，只有一个到内核级线程的编程接口。内核为进程及其内部的每个线程维护上下文信息，调度也是在内核基于线程架构的基础上完成。图2-2(b)说明了内核级线程的实现方式。

在一些系统中，使用组合方式的多线程实现。线程创建完全在用户空间中完成，线程的调度和同步也在应用程序中进行。一个应用程序中的多个用户级线程被映射到一些（小于或等于用户级线程的数目）内核级线程上。图2-2(c)说明了用户级与内核级的组合实现方式。

![](http://c.biancheng.net/cpp/uploads/allimg/140629/1-1406291220161Z.jpg)

#### 多线程模型

有些系统同时支持用户线程和内核线程由此产生了不同的多线程模型，即实现用户级线程和内核级线程的连接方式。

**1) 多对一模型**

将多个用户级线程映射到一个内核级线程，线程管理在用户空间完成。

此模式中，用户级线程对操作系统不可见（即透明）。

优点：线程管理是在用户空间进行的，因而效率比较高。

缺点：当一个线程在使用内核服务时被阻塞，那么整个进程都会被阻塞；多个线程不能并行地运行在多处理机上。

**2) 一对一模型**

将每个用户级线程映射到一个内核级线程。

优点：当一个线程被阻塞后，允许另一个线程继续执行，所以并发能力较强。

缺点：每创建一个用户级线程都需要创建一个内核级线程与其对应，这样创建线程的开销比较大，会影响到应用程序的性能。

**3) 多对多模型**

将 n 个用户级线程映射到 m 个内核级线程上，要求 m <= n。

特点：在多对一模型和一对一模型中取了个折中，克服了多对一模型的并发度不高的缺点，又克服了一对一模型的一个用户进程占用太多内核级线程，开销太大的缺点。又拥有多对一模型和一对一模型各自的优点，可谓集两者之所长。


### CPU调度              

**调度的基本概念**

在多道程序系统中，进程的数量往往多于处理机的个数，进程争用处理机的情况就在所难免。处理机调度是对处理机进行分配，就是从就绪队列中，按照一定的算法（公平、髙效）选择一个进程并将处理机分配给它运行，以实现进程并发地执行。

处理机调度是多道程序操作系统的基础，它是操作系统设计的核心问题。

![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p15.png)

#### 调度的时机、切换与过程

进程调度和切换程序是操作系统内核程序。当请求调度的事件发生后，才可能会运行进程调度程序，当调度了新的就绪进程后，才会去进行进程间的切换。理论上这三件事情应该顺序执行，但在实际设计中，在操作系统内核程序运行时，如果某时发生了引起进程调度的因素，并不一定能够马上进行调度与切换。


**进程调度方式**

所谓进程调度方式是指当某一个进程正在处理机上执行时，若有某个更为重要或紧迫的进程需要处理，即有优先权更髙的进程进入就绪队列，此时应如何分配处理机。

通常有以下两种进程调度方式：

1) 非剥夺调度方式，又称非抢占方式。是指当一个进程正在处理机上执行时，即使有某个更为重要或紧迫的进程进入就绪队列，仍然让正在执行的进程继续执行，直到该进程完成或发生某种事件而进入阻塞状态时，才把处理机分配给更为重要或紧迫的进程。

在非剥夺调度方式下，一旦把CPU分配给一个进程，那么该进程就会保持CPU直到终止或转换到等待状态。这种方式的优点是实现简单、系统开销小，适用于大多数的批处理系统，但它不能用于分时系统和大多数的实时系统。

2) 剥夺调度方式，又称抢占方式。是指当一个进程正在处理机上执行时，若有某个更为重要或紧迫的进程需要使用处理机，则立即暂停正在执行的进程，将处理机分配给这个更为重要或紧迫的进程。.

釆用剥夺式的调度，对提高系统吞吐率和响应效率都有明显的好处。但“剥夺”不是一种任意性行为，必须遵循一定的原则，主要有：优先权、短进程优先和时间片原则等。


**调度的基本准则**

1) CPU利用率。CPU是计算机系统中最重要和昂贵的资源之一，所以应尽可能使CPU 保持“忙”状态，使这一资源利用率最髙。

2) 系统吞吐量。表示单位时间内CPU完成作业的数量。长作业需要消耗较长的处理机时间，因此会降低系统的吞吐量。而对于短作业，它们所需要消耗的处理机时间较短，因此能提高系统的吞吐量。调度算法和方式的不同，也会对系统的吞吐量产生较大的影响。

3) 周转时间。是指从作业提交到作业完成所经历的时间，包括作业等待、在就绪队列中排队、在处迤机上运行以及进行输入/输出操作所花费时间的总和。

4) 等待时间。是指进程处于等处理机状态时间之和，等待时间越长，用户满意度越低。处理机调度算法实际上并不影响作业执行或输入/输出操作的时间，只影响作业在就绪队列中等待所花的时间。因此，衡量一个调度算法优劣常常只需简单地考察等待时间。

5) 响应时间。是指从用户提交请求到系统首次产生响应所用的时间。在交互式系统中，周转时间不可能是最好的评价准则，一般釆用响应时间作为衡量调度算法的重要准则之一。从用户角度看，调度策略应尽量降低响应时间，使响应时间处在用户能接受的范围之内。


### 操作系统典型调度算法

#### 先来先服务(FCFS)调度算法

![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p16.png)

#### 短作业优先(SJF)调度算法       

![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p17.png)

#### 高响应比优先调度算法            

![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p18.png)


#### 时间片轮转调度算法     

时间片轮转调度算法主要适用于分时系统。在这种算法中，系统将所有就绪进程按到达时间的先后次序排成一个队列，进程调度程序总是选择就绪队列中第一个进程执行，即先来先服务的原则，但仅能运行一个时间片，如100ms。在使用完一个时间片后，即使进程并未完成其运行，它也必须释放出（被剥夺）处理机给下一个就绪的进程，而被剥夺的进程返回到就绪队列的末尾重新排队，等候再次运行。

在时间片轮转调度算法中，时间片的大小对系统性能的影响很大。如果时间片足够大，以至于所有进程都能在一个时间片内执行完毕，则时间片轮转调度算法就退化为先来先服务调度算法。如果时间片很小，那么处理机将在进程间过于频繁切换，使处理机的开销增大，而真正用于运行用户进程的时间将减少。因此时间片的大小应选择适当。

时间片的长短通常由以下因素确定：系统的响应时间、就绪队列中的进程数目和系统的处理能力。

#### 最高优先级调度算法        

![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p19.png)


#### 多级反馈队列调度算法（集合了前几种算法的优点）

多级反馈队列调度算法是时间片轮转调度算法和优先级调度算法的综合和发展，如图2-5 所示。通过动态调整进程优先级和时间片大小，多级反馈队列调度算法可以兼顾多方面的系统目标。例如，为提高系统吞吐量和缩短平均周转时间而照顾短进程；为获得较好的I/O设备利用率和缩短响应时间而照顾I/O型进程；同时，也不必事先估计进程的执行时间。

![](http://c.biancheng.net/cpp/uploads/allimg/140629/1-140629155KQ11.jpg)
多级反馈队列调度算法的实现思想如下：

![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p20.png)

![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p21.png)


### 进程同步的基本概念         
在多道程序环境下，进程是并发执行的，不同进程之间存在着不同的相互制约关系。为了协调进程之间的相互制约关系，引入了进程同步的概念。

#### 临界资源

虽然多个进程可以共享系统中的各种资源，但其中许多资源一次只能为一个进程所使用，我们把一次仅允许一个进程使用的资源称为临界资源。许多物理设备都属于临界资源，如打印机等。此外，还有许多变量、数据等都可以被若干进程共享，也属于临界资源。

对临界资源的访问，必须互斥地进行，在每个进程中，访问临界资源的那段代码称为临界区。为了保证临界资源的正确使用，可以把临界资源的访问过程分成四个部分：

- 进入区。为了进入临界区使用临界资源，在进入区要检查可否进入临界区，如果可以进入临界区，则应设置正在访问临界区的标志，以阻止其他进程同时进入临界区。
- 临界区。进程中访问临界资源的那段代码，又称临界段。
- 退出区。将正在访问临界区的标志清除。
- 剩余区。代码中的其余部分。     

#### 同步

同步亦称直接制约关系，它是指为完成某种任务而建立的两个或多个进程，这些进程因为需要在某些位置上协调它们的工作次序而等待、传递信息所产生的制约关系。进程间的直接制约关系就是源于它们之间的相互合作。

例如，输入进程A通过单缓冲向进程B提供数据。当该缓冲区空时，进程B不能获得所需数据而阻塞，一旦进程A将数据送入缓冲区，进程B被唤醒。反之，当缓冲区满时，进程A被阻塞，仅当进程B取走缓冲数据时，才唤醒进程A。

#### 互斥

互斥亦称间接制约关系。当一个进程进入临界区使用临界资源时，另一个进程必须等待, 当占用临界资源的进程退出临界区后，另一进程才允许去访问此临界资源。

例如，在仅有一台打印机的系统中，有两个进程A和进程B，如果进程A需要打印时, 系统已将打印机分配给进程B,则进程A必须阻塞。一旦进程B将打印机释放，系统便将进程A唤醒，并将其由阻塞状态变为就绪状态。


### 实现临界区互斥的基本方法       

#### 软件实现方法

在进入区设置和检查一些标志来标明是否有进程在临界区中，如果已有进程在临界区，则在进入区通过循环检查进行等待，进程离开临界区后则在退出区修改标志。

**1) 算法一：单标志法。**

该算法设置一个公用整型变量turn,用于指示被允许进入临界区的进程编号，即若turn=0，则允许P0进程进入临界区。该算法可确保每次只允许一个进程进入临界区。但两个进程必须交替进入临界区，如果某个进程不再进入临界区了，那么另一个进程也将进入临界区（违背“空闲让进”）这样很容易造成资源利用的不充分。

**2) 算法二：双标志法先检查。**

该算法的基本思想是在每一个进程访问临界区资源之前，先查看一下临界资源是否正被访问，若正被访问，该进程需等待；否则，进程才进入自己的临界区。为此，设置了一个数据flag[i]，如第i个元素值为FALSE，表示Pi进程未进入临界区，值为TRUE，表示Pi进程进入临界区。

优点：不用交替进入，可连续使用；缺点：Pi和Pj可能同时进入临界区。按序列①②③④ 执行时，会同时进入临界区（违背“忙则等待”)。即在检查对方flag之后和切换自己flag 之前有一段时间，结果都检查通过。这里的问题出在检查和修改操作不能一次进行。

**3) 算法三：双标志法后检查。**

算法二是先检测对方进程状态标志后，再置自己标志，由于在检测和放置中可插入另一个进程到达时的检测操作，会造成两个进程在分别检测后，同时进入临界区。为此，算法三釆用先设置自己标志为TRUE后,再检测对方状态标志，若对方标志为TURE，则进程等待；否则进入临界区。


当两个进程几乎同时都想进入临界区时，它们分别将自己的标志值flag设置为TRUE，并且同时检测对方的状态（执行while语句），发现对方也要进入临界区，于是双方互相谦让，结果谁也进不了临界区，从而导致“饥饿”现象。


**4)算法四：Peterson’s Algorithm。**

为了防止两个进程为进入临界区而无限期等待，又设置变量turn，指示不允许进入临界区的进程编号，每个进程在先设置自己标志后再设置turn 标志，不允许另一个进程进入。这时，再同时检测另一个进程状态标志和不允许进入标志，这样可以保证当两个进程同时要求进入临界区，只允许一个进程进入临界区。


#### 硬件实现方法

本节对硬件实现的具体理解对后面的信号量的学习很有帮助。计算机提供了特殊的硬件指令，允许对一个字中的内容进行检测和修正，或者是对两个字的内容进行交换等。通过硬件支持实现临界段问题的低级方法或称为元方法。

**1) 中断屏蔽方法**

当一个进程正在使用处理机执行它的临界区代码时，要防止其他进程再进入其临界区访问的最简单方法是禁止一切中断发生，或称之为屏蔽中断、关中断。因为CPU只在发生中断时引起进程切换，这样屏蔽中断就能保证当前运行进程将临界区代码顺利地执行完，从而保证了互斥的正确实现，然后再执行开中断。

**2) 硬件指令方法**

TestAndSet指令：这条指令是原子操作，即执行该代码时不允许被中断。其功能是读出指定标志后把该标志设置为真。

硬件方法的优点：适用于任意数目的进程，不管是单处理机还是多处理机；简单、容易验证其正确性。可以支持进程内有多个临界区，只需为每个临界区设立一个布尔变量。

硬件方法的缺点：进程等待进入临界区时要耗费处理机时间，不能实现让权等待。从等待进程中随机选择一个进入临界区，有的进程可能一直选不上，从而导致“饥饿”现象。


### 信号量       
信号量机构是一种功能较强的机制，可用来解决互斥与同步的问题，它只能被两个标准的原语wait(S)和signal(S)来访问，也可以记为“P操作”和“V操作”。

原语是指完成某种功能且不被分割不被中断执行的操作序列，通常可由硬件来实现完成不被分割执行特性的功能。如前述的“Test-and-Set”和“Swap”指令，就是由硬件实现的原子操作。原语功能的不被中断执行特性在单处理机时可由软件通过屏蔽中断方法实现。

原语之所以不能被中断执行，是因为原语对变量的操作过程如果被打断，可能会去运行另一个对同一变量的操作过程，从而出现临界段问题。如果能够找到一种解决临界段问题的元方法，就可以实现对共享变量操作的原子性。


**利用信号量实现同步**

信号量机构能用于解决进程间各种同步问题。设S为实现进程P1、P2同步的公共信号量，初值为0。进程P2中的语句y要使用进程P1中语句x的运行结果，所以只有当语句x执行完成之后语句y才可以执行。其实现进程同步的算法如下：      

```
semaphore S = 0;  //初始化信号量
P1 ( ) {
    // …
    x;  //语句x
    V(S);  //告诉进程P2,语句乂已经完成
}
P2()）{
    // …
    P(S) ;  //检查语句x是否运行完成
    y;  // 检查无误，运行y语句
    // …
}
```

**利用信号量实现进程互斥**

信号量机构也能很方便地解决进程互斥问题。设S为实现进程Pl、P2互斥的信号量，由于每次只允许一个进程进入临界区，所以S的初值应为1（即可用资源数为1)。只需把临界区置于P(S)和V(S)之间，即可实现两进程对临界资源的互斥访问。其算法如下：

```
semaphore S = 1;  //初化信号量
P1 ( ) {
    // …
    P(S);  // 准备开始访问临界资源，加锁
    // 进程P1的临界区
    V(S);  // 访问结束，解锁
    // …
}
P2() {
    // …
    P(S); //准备开始访问临界资源，加锁
    // 进程P2的临界区；
    V(S);  // 访问结束，解锁
    // …
}
```


### 管程            

**管程的定义**

系统中的各种硬件资源和软件资源，均可用数据结构抽象地描述其资源特性，即用少量信息和对资源所执行的操作来表征该资源，而忽略了它们的内部结构和实现细节。管程是由一组数据以及定义在这组数据之上的对这组数据的操作组成的软件模块，这组操作能初始化并改变管程中的数据和同步进程。

**管程的组成**

1) 局部于管程的共享结构数据说明。

2) 对该数据结构进行操作的一组过程。

3) 对局部于管程的共享数据设置初始值的语句。

**管程的基本特性**

1) 局部于管程的数据只能被局部于管程内的过程所访问。

2) 一个进程只有通过调用管程内的过程才能进入管程访问共享数据。

3) 每次仅允许一个进程在管程内执行某个内部过程。

由于管程是一个语言成分，所以管程的互斥访问完全由编译程序在编译时自动添加，无需程序员关注，而且保证正确

![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p23.png) 


### 进程间通信IPC           

![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p24.png) 

