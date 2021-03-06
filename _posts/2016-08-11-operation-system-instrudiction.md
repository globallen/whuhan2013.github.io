---
layout: post
title: 操作系统概述
date: 2016-8-11
categories: blog
tags: [操作系统]
description: 操作系统
---



#### 操作系统的基本概念         

操作系统(Operating System, OS)是指控制和管理整个计算机系统的硬件和软件资源，并合理地组织调度计算机的工作和资源的分配，以提供给用户和其他软件方便的接口和环境的程序集合。计算机操作系统是随着计算机研究和应用的发展逐步形成并发展起来的，它是计算机系统中最基本的系统软件。

![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p1.png)

#### 操作系统的特征 

- 并发(Concurrence)                           
并发是指两个或多个事件在同一时间间隔内发生。操作系统的并发性是指计算机系统中同时存在多个运行着的程序，因此它具有处理和调度多个程序同时执行的能力。在操作系统 中，引入进程的目的是使程序能并发执行。

注意同一时间间隔（并发）和同一时刻（并行）的区别。在多道程序环境下，一段时间内，宏观上有多道程序在同时执行，而在每一时刻，单处理机环境下实际仅能有一道程序执行，故微观上这些程序还是在分时地交替执行。橾作系统的并发性是通过分时得以实现的。

注意，并行性是指系统具有可以同时进行运算或操作的特性，在同一时刻完成两种或两种以上的工作。并行性需要有相关硬件的支持，如多流水线或多处理机硬件环境。

- 共享（Sharing)                         
资源共享即共享，是指系统中的资源可供内存中多个并发执行的进程共同使用。共享可分为以下两种资源共享方式：

**互斥共享方式**            
系统中的某些资源，如打印机、磁带机，虽然它们可以提供给多个进程使用，但为使所打印或记录的结果不致造成混淆，应规定在一段时间内只允许一个进程访问该资源。

为此，当进程A访问某资源时，必须先提出请求，如果此时该资源空闲，系统便可将之分配给进程A使用，此后若再有其他进程也要访问该资源时（只要A未用完）则必须等待。仅当进程A访问完并释放该资源后，才允许另一进程对该资源进行访问。我们把这种资源共享方式称为互斥式共享，而把在一段时间内只允许一个进程访问的资源称为临界资源或独占资源。计算机系统中的大多数物理设备，以及某些软件中所用的栈、变量和表格，都属于临界资源，它们都要求被互斥地共享。

**同时访问方式**

系统中还有另一类资源，允许在一段时间内由多个进程“同时”对它们进行访问。这里所谓的“同时”往往是宏观上的，而在微观上，这些进程可能是交替地对该资源进行访问即 “分时共享”。典型的可供多个进程“同时”访问的资源是磁盘设备，一些用重入码编写的文件也可以被“同时”共享，即若干个用户同时访问该文件。

并发和共享是操作系统两个最基本的特征，这两者之间又是互为存在条件的：               
资源共享是以程序的并发为条件的，若系统不允许程序并发执行，则自然不存在资源共享问题；                
若系统不能对资源共享实施有效的管理，也必将影响到程序的并发执行，甚至根本无法并发执行。           


- 虛拟（Virtual)              
虚拟是指把一个物理上的实体变为若干个逻辑上的对应物。物理实体（前者）是实的，即实际存在的；而后者是虚的，是用户感觉上的事物。用于实现虚拟的技术，称为虚拟技术。在操作系统中利用了多种虚拟技术，分别用来实现虚拟处理器、虚拟内存和虚拟外部设备等。

在虚拟处理器技术中，是通过多道程序设计技术，让多道程序并发执行的方法，来分时使用一个处理器的。此时，虽然只有一个处理器，但它能同时为多个用户服务，使每个终端用户都感觉有一个中央处理器（CPU)在专门为它服务。利用多道程序设计技术，把一个物理上的CPU虚拟为多个逻辑上的CPU,称为虚拟处理器。

类似地，可以通过虚拟存储器技术，将一台机器的物理存储器变为虚拟存储器，以便从逻辑上来扩充存储器的容量。当然,这时用户所感觉到的内存容量是虚的。我们把用户所感觉到的存储器（实际是不存在的）称为虚拟存储器。

还可以通过虚拟设备技术，将一台物理I/O设备虚拟为多台逻辑上的I/O设备，并允许每个用户占用一台逻辑上的I/O设备，这样便可以使原来仅允许在一段时间内由一个用户访问的设备（即临界资源)，变为在一段时间内允许多个用户同时访问的共享设备。

因此，操作系统的虚拟技术可归纳为：时分复用技术，如处理器的分时共享；空分复用技术，如虚拟存储器（注：学到后续内容再慢慢领悟)。


- 随机        
在多道程序环境下，允许多个程序并发执行，但由于资源有限，进程的执行不是一贯到底，而是走走停停，以不可预知的速度向前推进，这就是进程的异步性。

异步性使得操作系统运行在一种随机的环境下，可能导致进程产生与时间有关的错误 (就像对全局变量的访问顺序不当会导致程序出错一样）。但是只要运行环境相同，操作系统必须保证多次运行进程，都获得相同的结果。这在第2章中会深入讨论。


#### 操作系统的目标和功能
为了给多道程序提供良好的运行环境，操作系统应具有以下几方面的功能：处理机管理、 存储器管理、设备管理和文件管理。为了方便用户使用操作系统，还必须向用户提供接口。同时操作系统可用来扩充机器，以提供更方便的服务、更高的资源利用率。

![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p2.png)


**操作系统作为用户与计算机硬件系统之间的接口**     

为方便用户使用计算机，操作系统还提供了用户接口。操作系统提供的接口主要分为两类•• 一类是命令接口，用户利用这些操作命令来组织和控制作业的执行；另一类是程序接口，编程人员可以使用它们来请求操作系统服务。

**操作系统用做扩充机器**

没有任何软件支持的计算机称为裸机，它仅构成计算机系统的物质基础，而实际呈现在用户面前的计算机系统是经过若干层软件改造的计算机。裸机在最里层，它的外面是操作系统，由操作系统提供的资源管理功能和方便用户的各种服务功能，将裸机改造成功能更强、 使用更方便的机器，通常把覆盖了软件的机器称为扩充机器，又称之为虚拟机。      

![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p3.png)


### 操作系统的发展与分类
![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p4.png)


#### 手工操作阶段（此阶段无操作系统）

用户在计算机上算题的所有工作都要人工干预，如程序的装入、运行、结果的输出等。随着计算机硬件的发展，人机矛盾（速度和资源利用）越来越大，必须寻求新的解决办法。

手工操作阶段有两个突出的缺点：                       
用户独占全机。不会出现因资源已被其他用户占用而等待的现象，但资源利用率低。               
CPU等待手工操作，CPU的利用不充分。                 

唯一的解决办法就是用高速的机器代替相对较慢的手工操作来对作业进行控制。            


#### 批处理阶段（操作系统开始出现）              
为了解决人机矛盾及CPU和I/O设备之间速度不匹配的矛盾，出现了批处理系统。它按发展历程又分为单道批处理系统、多道批处理系统（多道程序设计技术出现以后）。    

**1) 单道批处理系统**

系统对作业的处理是成批进行的，但内存中始终保持一道作业。该系统是在解决人机矛盾和CPU与I/O设备速率不匹配的矛盾中形成的。单道批处理系统的主要特征如下：     

- 自动性。在顺利的情况下，在磁带上的一批作业能自动地逐个依次运行，而无需人工干预。 '
- 顺序性。磁带上的各道作业是顺序地进入内存，各道作业的完成顺序与它们进入内存的顺序，在正常情况下应完全相同，亦即先调入内存的作业先完成。
- 单道性。内存中仅有一道程序运行，即监督程序每次从磁带上只调入一道程序进入内存运行，当该程序完成或发生异常情况时，才换入其后继程序进入内存运行。

此时面临的问题是：每次主机内存中仅存放一道作业，每当它运行期间（注意这里是“运行时”，并不是“完成后”）发出输入/输出请求后，高速的CPU便处于等待低速的I/O完成状态。为了进一步提高资源的利用率和系统的吞吐量，引入了多道程序技术。


**2) 多道批处理系统**

多道程序设计技术允许多个程序同时进入内存并运行。即同时把多个程序放入内存，并允许它们交替在CPU中运行，它们共享系统中的各种硬、软件资源。当一道程序因I/O请求而暂停运行时，CPU便立即转去运行另一道程序。它没有用某些机制提高某一技术方面的瓶颈问题，而是让系统的各个组成部分都尽量去“忙”，花费很少时间去切换任务，达到了系统各部件之间的并行工作，使其整体在单位时间内的效率翻倍。

多道程序设计的特点有：   

- 多道：计算机内存中同时存放多道相互独立的程序。
- 宏观上并行：同时进入系统的多道程序都处于运行过程中，即它们先后开始了各自的运行，但都未运行完毕。
- 微观上串行：内存中的多道程序轮流占有CPU，交替执行。

多道程序设计技术的实现需要解决下列问题：   

- 如何分配处理器。
- 多道程序的内存分配问题。
- I/O设备如何分配。
- 如何组织和存放大量的程序和数据，以便于用户使用和保证其安全性与一致性。

在批处理系统中釆用多道程序设计技术，就形成了多道批处理操作系统。该系统把用户提交的作业成批地送入计算机内存，然后由作业调度程序自动地选择作业运行。

优点是资源利用率高，多道程序共享计算机资源，从而使各种资源得到充分利用；系统吞吐量大，CPU和其他资源保持“忙碌”状态。缺点是用户响应的时间较长。不提供人机交互能力，用户既不能了解自己程序的运行情况，也不能控制计算机。


#### 分时操作系统         
![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p5.png)

#### 通用操作系统  
![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p6.png)


#### 实时操作系统         
为了能在某个时间限制内完成某些紧急任务而不需时间片排队，诞生了实时操作系统。这里的时间限制可以分为两种情况：如果某个动作必须绝对地在规定的时刻（或规定的时间范围）发生，则称为硬实时系统。例如，飞行器的飞行自动控制系统，这类系统必须提供绝对保证，让某个特定的动作在规定的时间内完成。如果能够接受偶尔违反时间规定，并且不会引起任何永久性的损害，则称为软实时系统，如飞机订票系统、银行管理系统。

在实时操作系统的控制下，计算机系统接收到外部信号后及时进行处理，并且要在严格的时限内处理完接收的事件。实时橾作系统的主要特点是及时性和可靠性。

#### 网络操作系统和分布式计算机系统

网络操作系统把计算机网络中的各台计算机有机地结合起来，提供一种统一、经济而有效的使用各台计算机的方法，实现各个计算机之间的互相传送数据。网络操作系统最主要的特点是网络中各种资源的共享以及各台计算机之间的通信。

分布式计算机系统是由多台计算机组成并满足下列条件的系统：系统中任意两台计算机通过通信方式交换信息；系统中的每一台计算机都具有同等的地位，即没有主机也没有从机； 每台计算机上的资源为所有用户共享；系统中的任意若千台计算机都可以构成一个子系统，并且还能重构；任何工作都可以分布在几台计算机上，由它们并行工作、协同完成。用于管理分布式计算机系统的操作系统称为分布式计算机系统。该系统的主要特点是：分布性和并行性。分布式操作系统与网络操作系统本质上的不同之处在于分布式操作系统中，若干台计算机相互协同完成同一任务。


### 操作系统的运行机制

计算机系统中，通常CPU执行两种不同性质的程序：一种是操作系统内核程序；另一种是用户自编程序或系统外层的应用程序。对操作系统而言，这两种程序的作用不同，前者是后者的管理者，因此“管理程序”要执行一些特权指令，而“被管理程序”出于安全考虑不能执行这些指令。所谓特权指令，是指计算机中不允许用户直接使用的指令，如I/O指令、 置中断指令，存取用于内存保护的寄存器、送程序状态字到程序状态字寄存器等指令。操作系统在具体实现上划分了用户态（目态）和核心态（管态)，以严格区分两类程序。


在软件工程思想和结构程序设计方法的影响下诞生的现代操作系统，几乎都是层次式的结构。操作系统的各项功能分别被设置在不同的层次上。一些与硬件关联较紧密的模块，诸如时钟管理、中断处理、设备驱动等处于最底层。其次是运行频率较髙的程序，诸如进程管理、存储器管理和设备管理等。这两部分内容构成了操作系统的内核。这部分内容的指令操作工作在核心态。

内核是计算机上配置的底层软件，是计算机功能的延伸。不同系统对内核的定义稍有区别，大多数操作系统内核包括四个方面的内容。

- 时钟管理
- 中断机制
- 原语
- 系统控制的数据结构及处理


### 中断和异常的概念     

在操作系统中引入核心态和用户态这两种工作状态后，就需要考虑这两种状态之间如何切换。操作系统内核工作在核心态，而用户程序工作在用户态。但系统不允许用户程序实现核心态的功能，而它们又必须使用这些功能。因此，需要在核心态建立一些“门”，实现从用户态进入核心态。在实际操作系统中，CPU运行上层程序时唯一能进入这些“门”的途径就是通过中断或异常。当中断或异常发生时，运行用户态的CPU会立即进入核心态，这是通过硬件实现的（例如，用一个特殊寄存器的一位来表示CPU所处的工作状态，0表示核心态，1表示用户态。若要进入核心态，只需将该位置0即可)。中断是操作系统中非常重要的一个概念，对一个运行在计算机上的实用操作系统而言，缺少了中断机制，将是不可想象的。

![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p7.png)

中断(Interruption)，也称外中断，指来自CPU执行指令以外的事件的发生，如设备发出的I/O结束中断，表示设备输入/输出处理已经完成，希望处理机能够向设备发下一个输入 / 输出请求，同时让完成输入/输出后的程序继续运行。时钟中断，表示一个固定的时间片已到，让处理机处理计时、启动定时运行的任务等。这一类中断通常是与当前程序运行无关的事件，即它们与当前处理机运行的程序无关。

异常(Exception)，也称内中断、例外或陷入(Trap)，指源自CPU执行指令内部的事件，如程序的非法操作码、 地址越界、算术溢出、虚存系统的缺页以及专门的陷入指令等引起的事件。对异常的处理一般要依赖于当前程序的运行现场，而且异常不能被屏蔽，一旦出现应立即处理。关于内中断和外中断的联系与区别如图1-2所示。

![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p8.png)


### 系统调用机制

所谓系统调用就是用户在程序中调用操作系统所提供的一些子功能，系统调用可以被看做特殊的公共子程序。系统中的各种共享资源都由操作系统统一掌管，因此在用户程序中，凡是与资源有关的操作（如存储分配、进行I/0传输以及管理文件等)，都必须通过系统调用方式向操作系统提出服务请求，并由操作系统代为完成。通常，一个操作系统提供的系统调用命令有几十乃至上百条之多。

![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p9.png)

显然，系统调用运行在系统的核心态。通过系统调用的方式来使用系统功能，可以保证系统的稳定性和安全性，防止用户随意更改或访问系统的数据或命令。系统调用命令是由操作系统提供的一个或多个子程序模块实现的。

这样，操作系统的运行环境可以理解为：用户通过操作系统运行上层程序（如系统提供的命令解释程序或用户自编程序)，而这个上层程序的运行依赖于操作系统的底层管理程序提供服务支持，当需要管理程序服务时，系统则通过硬件中断机制进入核心态，运行管理程序；也可能是程序运行出现异常情况，被动地需要管理程序的服务，这时就通过异常处理来进入核心态。当管理程序运行结束时，用户程序需要继续运行，则通过相应的保存的程序现场退出中断处理程序或异常处理程序，返回断点处继续执行。

![](https://raw.githubusercontent.com/whuhan2013/ImageRepertory/master/operation/p10.png)



### 重难点

#### 并行性与并发性的区别和联系

并行性和并发性是既相似又有区别的两个概念。并行性是指两个或多个事件在同一时刻发生。并发性是指两个或多个事件在同一时间间隔内发生。

在多道程序环境下，并发性是指在一段时间内，宏观上有多个程序在同时运行，但在单处理器系统中每一时刻却仅能有一道程序执行，故微观上这些程序只能是分时地交替执行。倘若在计算机系统中有多个处理器，则这些可以并发执行的程序便被分配到多个处理器上，实现并行执行，即利用每个处理器来处理一个可并发执行的程序。

#### 特权指令与非特权指令

所谓特权指令是指有特殊权限的指令，由于这类指令的权限最大，如果使用不当，将导致整个系统崩溃。比如：清内存、置时钟、分配系统资源、修改虚存的段表或页表、修改用户的访问权限等。如果所有的程序都能使用这些指令，那么你的系统一天死机《回就不足为奇了。为了保证系统安全，这类指令只能用于操作系统或其他系统软件，不直接提供给用户使用。因此，特权指令必须在核心态执行。实际上，CPU在核心态下可以执行指令系统的全集。形象地说，特权指令就是那些儿童不宜的东西，而非特权指令则是老少皆宜。

为了防止用户程序中使用特权指令，用户态下只能使用非特权指令，核心态下可以使用全部指令。当在用户态下使用特权指令时，将产生中断以阻止用户使用特权指令。所以把用户程序放在用户态下运行，而操作系统中必须使用特权指令的那部分程序在核心态下运行，保证了计算机系统的安全可靠。从用户态转换为核心态的唯一途径是中断或异常。

#### 访管指令与访管中断

访管指令是一条可以在用户态下执行的指令。在用户程序中，因要求操作系统提供服务而有意识地使用访管指令，从而产生一个中断事件（自愿中断），将操作系统转换为核心态，称为访管中断。访管中断由访管指令产生，程序员使用访管指令向操作系统请求服务。

为什么要在程序中引入访管指令呢？这是因为用户程序只能在用户态下运行，如果用户程序想要完成在用户态下无法完成的工作，该怎么办？解决这个问题要靠访管指令。访管指令本身不是特权指令，其基本功能是让程序拥有“自愿进管”的手段，从而引起访管中断。

当处于用户态的用户程序使用访管指令时，系统根据访管指令的操作数执行访管中断处理程序，访管中断处理程序将按系统调用的操作数和参数转到相应的例行子程序。完成服务功能后，退出中断，返回到用户程序断点继续执行。


**参考链接**    

[计算机操作系统教程：介绍现代操作系统原理及应用](http://c.biancheng.net/cpp/u/xitong/)

