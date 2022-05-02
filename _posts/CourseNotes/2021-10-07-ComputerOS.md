---
title: 计算机操作系统学习笔记
categories: CourseNotes
---
# Chapter 1: Overview

Part 1 Operating System Overview

Part 2 Processes and Scheduling 进程与调度

Part 3 Memory Management 存储管理

Part 4 Device Management 设备管理

Part 5 File Management 文件管理

 ![Screenshot from 2021-10-07 14-30-31](Pics_ComputerOS/Screenshot from 2021-10-07 14-30-31.png)

## Learning Objectives:

1. Explain what is OS
2. 分析OS的功能性与非功能性需求
3. Simple Batch Systems: 单道批处理系统（道指进程或者线程的数量）&Batch Multiprogramming, Uniprogramming & Multiprogramming, Time Sharing
4. Concurrency & Parallelism 并发&并行
5. Models of Executions: Single-Mode, Multi-Mode. Dual-Mode
6. Microkernels 微核结构 

## What is OS

四种观点：

1. User/Computer Interface: OS是计算机用户使用计算机系统的接口，它为计算机用户提供了方便的工作环境。
2. Virtual Machine: OS是建立在计算机硬件平台上的虚拟机器，它为应用软件提供了许多比计算机硬件功能更强或计算机没有的功能。（解释：例如计算机硬件只能串行地处理程序，但通过操作系统可以实现并行处理，这就是实现了更强的功能）
3. Resource Manager: OS是计算机系统中各类资源的管理者，负责分配、回收以及控制系统中的各种软硬件资源。（本课程按照此观点讲解）
4. Job Organizer: OS是计算机系统工作流程的组织者，负责协调在系统中运行的各个应用软件的运行次序。

## OS的系统需求（按照软件工程的观点分析）

系统需求：

1. 软件系统能提供的服务（功能性需求）
2. 软件系统在提供这些服务时，需要满足的限制条件（非功能性需求）
3. 软件系统具有适应某些变化的能力（非功能性需求）

### 功能性需求：

计算机用户需要的用户命令、用户接口

​	命令的使用方式：脱机使用方式(off-line，不受系统控制)，联机使用方式(on-line，受系统控制)

应用软件需要的System Call（系统调用）、程序接口/应用编程接口(API)

​	当前常用的两种API：POSIX.1、WIN32 API

### 非功能性需求：

**Performance性能 or Efficiency效率**

​	maximize throughput 最大化系统吞吐量（在一定时间内完成尽可能多的任务）minimize response time（最小化响应时间）In the case of time sharing, accommodate as many users as possible（在分时的情况下，满足尽可能多的用户）

**Fairness公平性**

​	equal and fair access to all processes

**Reliability可靠性**

**Security安全性**

**Scalability可伸缩性**

**Extensibility可扩展性**

**Portability可移植性**

### OS对硬件平台的依赖

Timer, I/O interrupts, 

DMA(直接存储存取) or Channel, 

Privilege Instructions(硬件支撑的特权指令),

Memory Protection Mechanism (MMU，虚拟地址转成实际地址，通过硬件实现效率更高)

### 一些基本概念

**Job作业**

- 计算机用户在一次上机过程中要求计算机系统为其所做的工作的集合，作业中的每项相对独立的工作称为作业步

- 作业的基本类型：Off-line Job用户不能在此类作业被处理时改变已定义好的 作业步， On-line Job可以改变作业步

**Thread & Process** 

Thread线程：程序的一次相对独立的运行过程；在现代OS中，线程是系统调度的最小单位

Process进程：系统分配资源的基本对象；在现代OS中，进程仅仅是系统中拥有资源的最小实体；在传统OS中，进程同时也是系统调度的最小单位

线程归属于某些进程，一个进程中有多个线程，线程可以用进程的资源，但是调度执行时是以线程为单位。

**Virtual Memory & File**

Virtual Memory 虚拟存储：进程的逻辑地址空间，是现代OS对计算机系统中多级物理存储体系进行高度抽象的结果（把更便宜的外存虚拟为内存，从而扩展内存的空间）

File：命名了的字节流，是现代OS对计算机系统中种类繁多的外部设备进行高度抽象的结果



## OS的演变、类型及特征

### Serial Processing(串行处理)

- No OS
- 通过按钮、开关、指示灯完成输入和显示工作
  - 浪费时间
  - Setup包括加载编译器、源代码、保存编译后的代码、加载和链接，都是串行、手动完成的，低效

### Simple Batch Systems(简单批处理系统)

Jobs在外存排队，Monitor把作业从外存放到内存，优先选择哪个作业就涉及到调度问题

- Monitors:控制running programs的软件
  - 在main memory（内存）并且可以执行
- Batch jobs together
- Program branches back to monitor when finished

在程序处理完成，进行IO的时候，cpu就空闲出来，这就出现了多道处理的可能。

![Screenshot from 2021-10-07 17-28-32](Pics_ComputerOS/Screenshot from 2021-10-07 17-28-32.png)

### Multiprogramming(多道程序设计)

（多道程序设计又会涉及到效率提高的问题，安全性问题等等）    

![Screenshot from 2021-10-07 19-28-07](Pics_ComputerOS/Screenshot from 2021-10-07 19-28-07.png)

**Difficulties:**

- Improper synchronization（同步或者说协调）
  - 确保等待IO的设备能够收到signal来提醒什么时候工作
- Failed mutual exclusion（互斥）
  - 比如两个程序同时对一个文件进行写操作
- Nondeterminate program operation（操作的不确定性）
  - program should only depend on input to it
- Deadlocks（死锁）
  - 大家都在竞争资源，都不放，导致死锁



### Time Sharing (分时系统)

根据时间分配系统资源，第一个作业用10ms，第二个作业用10ms...

- 用multiprogramming处理多个交互式作业（对单个作业没有意义，频繁中断反而降低效率）
- 处理器的时间由多个users分享
- 多个user同时通过多个终端访问系统

![Screenshot from 2021-10-07 20-21-21](Pics_ComputerOS/Screenshot from 2021-10-07 20-21-21.png)

### 现代OS的基本类型及基本特征

**按硬件平台系统结构分类**

- 单机OS（本课程）
- 并行OS
- 网络OS
- 分布式OS

**按功能特征分类**

- Batch_Processing OS （批处理系统）
  - 强调吞吐量大，一批作业要尽快完成
- Time_Sharing OS（分时系统）
  - 多交互式用户，使用户感觉到响应速度很快
- Real_Time OS（实时系统）
  - 实时控制系统、实时信息处理系统

**现代OS的两个基本特征**

- 任务共行
  - 宏观上看，指多个任务共同运行
  - 微观上看，指单处理机系统中的任务并发（Task Concurrency，即多个任务在单个处理机上交替运行）或多处理机系统中的任务并行（Task Parallelism，即多个任务在多个处理机上同时运行）
- 资源共享
  - 宏观上看，指多个任务可以同时使用系统中的软硬件资源
  - 微观上看，指多个任务可以交替互斥地使用系统中的某个资源

**任务管理模型**

Task：计算机系统在某个资源集合上所做的一次相对独立的计算过程

- 现代OS中，任务用线程和进程这两个基本概念共同表示；传统OS中，任务仅仅用进程这一基本概念表示
- 现代OS中，任务管理模型用线程状态转换图表示；传统OS中，任务管理模型用进程状态转换图表示

**资源管理模式**

Resource：程序和数据组成的软件资源以及CPU、存储器、IO设备等硬件资源

- 通常情况下，系统用**竞争模式**管理**软件资源**；为此，系统将为共享同一软件资源的多个任务提供互斥机制
- 对**硬件资源**，系统常用**分配模式**加以管理。该模式可描述为：申请——分配——使用——释放——回收



## OS Architecture常见的OS体系结构

### 一种常见的OS总体结构风格

现代OS总体结构一般包含两类子系统

- 用户接口子系统：提供计算机需求的用户命令
- 基础平台子系统：提供应用软件需求的系统调用

二者关系具有单向性，前者能够引用后者所提供的各种系统调用，后者不会引用前者的各种用户命令

### OS基础平台子系统结构风格

- Layered Structural Style（分层结构风格）
  - 包含若干层(layer)，每一层实现一组基本概念以及与其相关的基本属性
  - 层与层之间的相互关系：所有各层的实现不依赖其以上各层所提供的概念及其属性，只依赖其**直接下层**所提供的概念以及属性
- Hierarchical Structural Style（分级结构风格）
  - 包含若干级(level)，每一级实现一组基本概念以及与其相关的基本属性
  - 级与级之间的相互关系：所有各级的实现不依赖其以上各级所提供的概念及其属性，只依赖其**以下各级**所提供的概念以及属性
- Modular Structural Style（分块结构风格）
  - 包含若干块(module)，每一块实现一组基本概念以及与其相关的基本属性
  - 块与块之间的相互关系：所有各级的实现**均可以任意引用其他各块**所提供的概念以及属性

![Screenshot from 2021-10-07 21-03-52](Pics_ComputerOS/Screenshot from 2021-10-07 21-03-52.png)

---

按照模式分类（主要区别于系统的保护模式，有还是没有，又或是有多种保护模式），模式就是程序在运行过程中是用的、由硬件体系结构提供的CPU特权模式，出于安全考虑要给cpu特权。

- Multi-Mode Structural Style(多模式结构风格)
  - 多个模式模块，这些模式模块或者是一个应用软件或者是一个基础平台子系统的一部分
  - 不同的模式模块在不同的CPU特权模式下运行（保护的级别不一样）

- Single-Mode Structural Style(单模式结构风格)
  - 一个模式模块，该模式模块由应用软件和基础平台子系统共同组成
  - 应用软件和基础平台子系统在同一CPU特权模式下运行



![Screenshot from 2021-10-07 21-45-12](Pics_ComputerOS/Screenshot from 2021-10-07 21-45-12.png)



### 双模式基础平台子系统结构风格

既安全又不损失太多系统资源。

包含两个模式模块，它们分别在两种不同的CPU特权模式下运行

- 核外子系统，CPU特权模式称为User Mode
- 核心子系统，CPU特权模式称为Kernel Mode

![Screenshot from 2021-10-07 21-49-28](Pics_ComputerOS/Screenshot from 2021-10-07 21-49-28.png)

### Microkernels（微核）结构

![Screenshot from 2021-10-07 21-54-34](Pics_ComputerOS/Screenshot from 2021-10-07 21-54-34.png)

# Chapter 2: 进程与调度 

## 1. 进程的描述与控制

### 进程概念与进程并发执行

- 程序顺序执行时的特征：顺序性、封闭性、可再现性
- 程序并发执行时的特征：间断性、非封闭性、不可再现性
- 程序并发执行条件（Bernstein条件）：R(P1)∩W(P2)∪W(P1)∩R(P2)∪W(P1)∩W(P2)={} R = read; W = write，也就是说两个进程的读写之间不能有交集

- 进程的概念
  - Characteristics of Process: Dynamic动态性， Concurrency并发性，Independent独立性，Asynchronous异步性
  - Process Structure: Programs, Datas, PCB(Process Control Block)



### 进程状态转换

- 进程状态与状态转换 Process States
  - 进程的并发执行
  - 进程的2状态: Running, Non-running
  - 进程的5状态: Running执行（占用处理机）, Ready就绪（准备执行）, Blocked阻塞（等待某事件发生）, New新状态（进程已经创建，但没有被OS接纳为可执行进程）, Exit退出（取消或停止，将死状态）
  - 进程状态转换图

![Screenshot from 2021-10-08 22-01-15](Pics_ComputerOS/Screenshot from 2021-10-08 22-01-15.png)

- 7状态：

  - Swapping 对换技术、交换技术：将内存中暂时不能运行的进程或暂时不用的程序和数据，Swapping-out到外存，以腾出足够的内存空间，把已具备运行条件的进程或进程需要的数据和程序，Swapping-in到内存 
  - 挂起状态 Suspended Processes：将正在等待I/O的程序交换到disk来清理出更多的内存，于是Block state的程序在被交换到外存之后就变成了Suspended state。当然，导致挂起的原因有很多

  <img src="Pics_ComputerOS/Screenshot from 2021-10-09 16-33-50.png" alt="Screenshot from 2021-10-09 16-33-50" style="zoom:50%;" />

  - 于是又有新的状态出现：
    - Ready, Suspend：进程在外存，只要调入内存即可执行
    - Blocked, Suspend：进程在外存，等待事件
    - Blocked, Suspend到Ready, Suspend是可以实现的，通过PCB完成

![Screenshot from 2021-10-09 16-39-26](Pics_ComputerOS/Screenshot from 2021-10-09 16-39-26.png)



### 进程的描述和结构

Operating System Control Structures

- 每个进程和资源的现在状态信息
- OS管理的每个个体的表
  - Memory Tables
    - Allocation of main memory to processes 给进程的内存分配情况
    - Allocation of secondary memory to processes 进程在外存的存储位置
    - Protection attributes for access to shared memory regions 访问共享内存区的保护属性
    - Information needed to manage virtual memory 管理虚拟存储需要的信息
  - I/O Tables
    - I/O device is available or assigned IO设备是空闲的还是已经被分配的
    - Status of I/O operation IO操作的状态
    - Location in main memory being used as the source or destination of tne I/O transfer 内存空间中IO传输的源和目的地位置
  - File Tables
    - Existence of files 
    - Location on secondary memory 外存中的位置
    - Current Status
    - Attributes 文件的属性
    - 一般在文件管理系统里维护
  - Process Tables
    - Where process is located 进程的位置
    - Attributes necessary for its management
      - Process ID
      - Process state
      - Location in memory

> Process Location
>
> - 进程包括很多需要执行的程序
>   - Data locations for local and global variables
>   - Any defined constants
>   - Stack
> - Process control block (PCB)
>   - Collection of attributes
> - Process image 进程映像，一个进程的所有信息
>   - Collection of program, data, stack, PCB and attributes



- PCB (Process Control Block)分析一个操作系统，首先就要分析PCB
  - OS控制和管理进程时所用的基本数据结构
  - 作用：PCB是相关进程存在于系统中的唯一标志；系统根据PCB而感知相关进程的存在
  - 内容：通常包括Identifiers、状态、控制、指针等多种信息
    - Process identification：Identifiers of 进程ID，父进程ID，用户ID
    - Processor State Information: 
      - User-Visible Registers用户可见寄存器
      - Control and Status Register: Program counter, Condition codes, Status information
      - Stack Pointer堆栈指针
    - Process Control Information
      - Scheduling and State Information: Process state, Priority, Scheduling-related information, Event
      - Data Structuring
      - Interprocess Communication
      - Process Privileges
      - Memory Management
      - Resource Ownership and Utilization

进程的逻辑抽象就叫进程映像

![Screenshot from 2021-10-12 11-36-39](Pics_ComputerOS/Screenshot from 2021-10-12 11-36-39.png)

### 操作系统如何控制系统进程的并发执行

**Typical Function of an OS Kernel **

-->作业：操作系统是如何提供这些功能的？他自己是一个进程吗？以什么方式来提供这些服务？

**Function 1: 资源管理功能**

- Process Management: 进程创建和终止、调度、状态转换、同步和通信、管理PCB
- Memory Management: 为进程分配地址空间、对换、段/页管理
- I/O Management: 缓存管理、为进程分配I/O通道和设备

**Function 2: 支撑功能**

- Interrupt handling: 中断处理
- Timing: 时钟管理
- Primitive(原语): Atomic Operation 原语的操作不可中断
- Accounting: 统计
- Monitoring: 监测



**Process Control**

Primitives 原语：最小的进程单位，不可中断

- Process Switch
  - When to Switch a Process
    - Clock interrupt: Process执行的时间超过最大的时间片了(time slice)
    - I/O interrupt
    - Memory fault
    - Trap: error occured
    - Supervisor call: 如打开文件
  - Change of a Process State
    - Save context of processor including program counter and other registers
    - Update the PCB of the process that is currently running
    - Move PCB to appropriate queue - ready, blocked
    - Select another process for execution
    - Update the PCB of the process selected
    - Update memory-management data structures
    - Restore context of selected process
  - Process Switching vs. Mode Switching（这个模式是指双模式单模式的那个模式，见第一章最后）
    - Process Switch 是作用于进程之间的一种操作。当分派程序收回当前进程的CPU并准备把它分派给某个就绪进程时，该操作将被引用
    - Mode Switch 是进程内部所引用的一种操作。当进程映像所包含的程序引用核心子系统所提供的系统调用时，该操作将被引用。
    - 进程切换的时候要用到调度程序，而调度程序是系统的，所以要进入系统态下，中断也是。因此Process Switch的时候是会涉及到Mode Switch的，但是Mode Switch不一定有Process Switch（要看操作系统本质是不是一个进程）
- Create and Terminate
  - When Process Creation 
    - Submission of a batch job
    - User logs on用户注册
    - Created to provide a service such as printing
    - Process creates another process
  -  create()原语，步骤
    - 为进程分配一个唯一标识号ID
    - 为进程分配空间
    - 初始化PCB：进程标识，处理机状态信息，进程状态
    - 建立链接 ：若调度队列是链表，则将新进程插入到就绪或挂起链表
    - 建立或扩展其他数据结构
  - When Process Termination
    - Batch job issues Halt instruction
    - User logs off
    - Quit an application
    - Error and fault conditions
  - Reasons for Process Termination
    - Normal completion 正常结束
    - Time limit exceeded 超时终止
    - Memory unavailable 内存不足
    - Bounds violation 越界访问
    - Protection error 企图使用未允许用的数据，或操作方式错
    - Arithmetic error 计算错，如除0，企图存储硬件允许的最大数
    - Time overrun 超时等待某事件发生
    - I/O failure 找不到文件无法读写文件等
    - Invalid instruction 企图执行不存在的指令
    - 等等等等......
  - destroy() 原语，步骤
    - 根据被终止进程的ID，找到其PCB，读出该进程的状态
    - 若为执行状态，则终止，调度新进程执行
    - 若有子孙进程，立即终止
    - 全部资源归还父进程或系统
    - 将被终止进程的PCB从所在队列移出，等待其他程序来收集信息（将死状态）
- Block and Wakeup
  - Why Block
    - 请求系统服务
    - 启动某种操作
    - 新数据尚未到达
    - 无新工作可做
  - 原语
    - 阻塞原语block()：当出现阻塞事件，进程调用阻塞原语将自己阻塞，状态变为“阻塞状态”，进入相应阻塞队列
    - 唤醒原语wakeup()：当阻塞进程期待的事件发生，有关进程调用唤醒原语将其唤醒，状态变为Ready，进入就绪队列
- Suspend and Activate 基于交换技术Swapping
  - 挂起原语suspend()
  - 激活原语active()

### **Thread线程**

进程是资源分配单位，线程是调度单位；线程不再拥有大量资源，是共享了进程的资源

**线程都有什么？**

- An execution state (running, ready, etc.)
- Saved thread context when not running 
- Has an execution stack 执行堆栈
- Some per-thread static storage for local variables
- Access to the memory and resources of its process
  - all threads of a process share this

![Screenshot from 2021-10-18 21-56-15](Pics_ComputerOS/Screenshot from 2021-10-18 21-56-15.png)



**Benefits of Threads线程的优势**

- 创建线程比创建进程需要的时间更少
- 终止的时间也更短
- 同一进程内的线程之间切换所需的时间更短
- 同一进程的线程是共享内存和文件的，所以他们之间的交流不需要唤起内核（不需要模式切换）

**Threads**

- Suspending a process involves suspending all threads of the process
- Termination of a process, terminates all threads within the process

**Multithreading**

- Operating system supports multiple threads of execution within a single process.
- MS-DOS supports a single thread. 单线程
- UNIX supports multiple user processes but only supports one thread per process. 单进程单线程（现代UNIX系统还是多线程的）

> UNIX: 多用户多进程系统，每个进程又支持多线程
>
> Windows. Linux: 多用户多任务系统，任务通常指线程，因此一般是单进程多线程系统









# Bottom









