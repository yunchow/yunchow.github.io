---
layout: post
title: Linux/Java 性能优化
categories: [技术]
description: Linux Java性能优化
keywords: Java性能优化, Linux性能优化
---

> 工欲善其事，必先利其器
> 
> 做应用性能优化或者找出性能瓶颈如果没有好用的工具是无法想象的
> 

这里汇总记录了工作中常用的性能优化工具，持续更新

Java 应用常见的性能优化包括了如下几大方面

* JVM
* Linux
* 数据库
* 中间件

所以下面会对这几类分开进行讲解，最后还列举了常见的一些错误供参考


# JVM 篇

## jps
jps 用于查看系统中的 Java 进程，作用等同于 ps -ef | grep java

#### jps -h
查看帮助信息

## jmap
jmap 用于查看 JVM 堆内存的情况，包括了堆内存汇总、对象大小、类加载、永久代（方法区，常量池）等相关信息，jmap 还可以 dump 堆快照到一个二进制文件进行离线分析。

#### jmap -heap pid
打印进程堆内存的汇总信息，这是一个概要的输出，可以看到当前的 JVM 用的是哪个 GC 算法和一些简单的配置比例参数，还可以看到堆内存的分区和使用情况，一个典型的 JVM6 的输出是这样的

**注意：这个命令可能会导致 Java 进程挂起**

``` 
Attaching to process ID 193350, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 20.0-b12-internal

using parallel threads in the new generation.
using thread-local object allocation.
Concurrent Mark-Sweep GC

Heap Configuration:
   MinHeapFreeRatio = 40
   MaxHeapFreeRatio = 70
   MaxHeapSize      = 4294967296 (4096.0MB)
   NewSize          = 2147483648 (2048.0MB)
   MaxNewSize       = 2147483648 (2048.0MB)
   OldSize          = 5439488 (5.1875MB)
   NewRatio         = 2
   SurvivorRatio    = 10
   PermSize         = 100663296 (96.0MB)
   MaxPermSize      = 402653184 (384.0MB)

Heap Usage:
New Generation (Eden + 1 Survivor Space):
   capacity = 1968570368 (1877.375MB)
   used     = 1301671664 (1241.370834350586MB)
   free     = 666898704 (636.0041656494141MB)
   66.12268909251407% used
Eden Space:
   capacity = 1789657088 (1706.75MB)
   used     = 1219023832 (1162.5517196655273MB)
   free     = 570633256 (544.1982803344727MB)
   68.11493889940104% used
From Space:
   capacity = 178913280 (170.625MB)
   used     = 82647832 (78.8191146850586MB)
   free     = 96265448 (91.8058853149414MB)
   46.19435292897207% used
To Space:
   capacity = 178913280 (170.625MB)
   used     = 0 (0.0MB)
   free     = 178913280 (170.625MB)
   0.0% used
concurrent mark-sweep generation:
   capacity = 2147483648 (2048.0MB)
   used     = 1620385664 (1545.3201904296875MB)
   free     = 527097984 (502.6798095703125MB)
   75.45508742332458% used
Perm Generation:
   capacity = 399908864 (381.3828125MB)
   used     = 244258792 (232.9433364868164MB)
   free     = 155650072 (148.4394760131836MB)
   61.078614151448264% used
```

#### jmap -histo pid
打印 Java 对象占用的堆内存详细列表，会按占用大小降序排列

```
num     #instances         #bytes  class name
----------------------------------------------
   1:       2057349      166008824  [C
   2:        464806      113522760  [B
   3:        171908       91584528  [I
   4:       2412542       77201344  java.lang.String
   5:       1979521       63344672  java.util.HashMap$Entry
   6:       1550946       62037840  java.util.concurrent.ConcurrentHashMap$Segment
   7:        868145       57148928  [Ljava.lang.Object;
   8:        365993       55979392  <constMethodKlass>
   9:       1636340       52362880  java.util.concurrent.locks.ReentrantLock$NonfairSync
  10:        365993       43930904  <methodKlass>
  11:       1551930       38532624  [Ljava.util.concurrent.ConcurrentHashMap$HashEntry;
  12:         30900       37707912  <constantPoolKlass>
  13:        328579       34937280  [Ljava.util.HashMap$Entry;
  14:         30900       24044144  <instanceKlassKlass>
  15:        385746       23722992  <symbolKlass>
  16:         22361       19328328  <constantPoolCacheKlass>
  17:        796899       19125576  java.lang.Long
  18:        637196       15292704  java.util.ArrayList
  19:        287337       13792176  java.util.HashMap
  20:        219462       12289872  net.sf.cglib.asm.Item
  21:        175085       11205440  net.sf.cglib.asm.Label
  22:        366987        8807688  net.sf.cglib.asm.Edge
  23:        204301        8172040  java.util.HashMap$KeyIterator
  24:         97944        7770912  [Ljava.util.concurrent.ConcurrentHashMap$Segment;
  25:        304873        7316952  org.h2.value.ValueLong
  26:        142069        6928760  [Lorg.h2.value.Value;
  27:        168645        6745800  java.util.LinkedHashMap$Entry
```

#### jmap -permstat pid
用来搜集持久代的统计信息，执行时间很慢

#### jmap -dump:format=b,file=heap.bin pid
导出堆内存的快照，便于对内存进行离线分析，但是需要借助一些第三方的工作，比如：MAT, Jprofiler

* 堆内存概要汇总信息

![堆内存概要汇总信息](/imgs/heap_sum.png)

* 堆内存详细信息

![堆内存详细信息](/imgs/heap_histo.png)

## jhat
对导出的 Java 堆文件进行在线的分析，功能类似 MAT

```
jhat -J-mx512m -port 7000 <file>
```
这样开启服务后，可以在本地通过浏览器进行访问

## jstat
查看 JVM 运行时的一些信息

#### jstat -gcutil pid 1000

每秒打印出 GC 情况，如下

```
./jstat -gcutil 193350 1000
  S0     S1     E      O      P     YGC     YGCT    FGC    FGCT     GCT
 57.64   0.00  26.82  75.46  61.13   1202   63.121     8    2.563   65.684
 57.64   0.00  26.82  75.46  61.13   1202   63.121     8    2.563   65.684
 57.64   0.00  26.82  75.46  61.13   1202   63.121     8    2.563   65.684
 57.64   0.00  26.82  75.46  61.13   1202   63.121     8    2.563   65.684
 57.64   0.00  26.82  75.46  61.13   1202   63.121     8    2.563   65.684
 57.64   0.00  26.82  75.46  61.13   1202   63.121     8    2.563   65.684
```

## jstack

# Linux 篇

## vmstat

## iostat

## dmesge

## free

## uptime

## top

## df / du

# 数据库篇

## Mysql 索引优化

# 中间件篇

## Tomcat

# 其他工具

### shell 循环工具

 工作中常常需要去遍历一个文件，针对每一行记录做一些数据订正或者查询的处理。比如说根据一个包含了订单号列表的文件批量执行快速退款操作。

``` shell
for i in `cat oid.txt`; do curl -d "outerId=$i&bizType=3001" localhost:7001/tool/query/simple3 >> result.txt;echo " " >> result.txt;done
```

# 典型错误

## OOM
OutOfMemoryError 异常除了程序计数器外，虚拟机内存的其他几个运行时区域都有发生 OutOfMemoryError(OOM) 异常的可能

#### Java Heap 溢出
一般的异常信息：java.lang.OutOfMemoryError:Java heap spacess

java堆用于存储对象实例，我们只要不断的创建对象，并且保证GC Roots到对象之间有可达路径来避免垃圾回收机制清除这些对象，就会在对象数量达到最大堆容量限制后产生内存溢出异常。
出现这种异常，一般手段是先通过内存映像分析工具(如Eclipse Memory Analyzer)对dump出来的堆转存快照进行分析，重点是确认内存中的对象是否是必要的，先分清是因为内存泄漏(Memory Leak)还是内存溢出(Memory Overflow)。
如果是内存泄漏，可进一步通过工具查看泄漏对象到GC Roots的引用链。于是就能找到泄漏对象时通过怎样的路径与GC Roots相关联并导致垃圾收集器无法自动回收。
如果不存在泄漏，那就应该检查虚拟机的参数(-Xmx与-Xms)的设置是否适当。

#### 虚拟机栈和本地方法栈溢出
如果线程请求的栈深度大于虚拟机所允许的最大深度，将抛出StackOverflowError异常。
如果虚拟机在扩展栈时无法申请到足够的内存空间，则抛出OutOfMemoryError异常
这里需要注意当栈的大小越大可分配的线程数就越少。

#### 运行时常量池溢出
异常信息：java.lang.OutOfMemoryError:PermGen space

如果要向运行时常量池中添加内容，最简单的做法就是使用String.intern()这个Native方法。该方法的作用是：如果池中已经包含一个等于此String的字符串，则返回代表池中这个字符串的String对象；否则，将此String对象包含的字符串添加到常量池中，并且返回此String对象的引用。由于常量池分配在方法区内，我们可以通过-XX:PermSize和-XX:MaxPermSize限制方法区的大小，从而间接限制其中常量池的容量。

#### 方法区溢出
方法区用于存放Class的相关信息，如类名、访问修饰符、常量池、字段描述、方法描述等。

异常信息：java.lang.OutOfMemoryError:PermGen space

方法区溢出也是一种常见的内存溢出异常，一个类如果要被垃圾收集器回收，判定条件是很苛刻的。在经常动态生成大量Class的应用中，要特别注意这点。

## 堆内存溢出
#### Java.lang.OutOfMemoryError: Java heap space
这种是java堆内存不够，一个原因是真不够，另一个原因是程序中有死循环；
如果是java堆内存不够的话，可以通过调整JVM下面的配置来解决：

```
-Xms4096M
-Xmx4096M
```

#### java.lang.OutOfMemoryError: GC overhead limit exceeded
JDK6新增错误类型，当GC为释放很小空间占用大量时间时抛出；一般是因为堆太小，导致异常的原因，没有足够的内存。

* 查看系统是否有使用大内存的代码或死循环
* 通过添加JVM配置，来限制使用内存

```
-XX:-UseGCOverheadLimit
```

#### java.lang.OutOfMemoryError: PermGen space
这种是P区内存不够，可通过调整JVM的配置
```
-XX:MaxPermSize=128m
-XXermSize=128m
```

JVM的Perm区主要用于存放Class和Meta信息的,Class在被Loader时就会被放到PermGen space，这个区域成为年老代，GC在主程序运行期间不会对年老区进行清理，默认是64M大小，当程序需要加载的对象比较多时，超过64M就会报这部分内存溢出了，需要加大内存分配，一般 128m 足够。 

#### java.lang.OutOfMemoryError: Direct buffer memory
调整 -XX:MaxDirectMemorySize= 参数，如添加JVM配置
```
-XX:MaxDirectMemorySize=128m
```

#### java.lang.OutOfMemoryError: unable to create new native thread
Stack空间不足以创建额外的线程，要么是创建的线程过多，要么是Stack空间确实小了。 

由于JVM没有提供参数设置总的stack空间大小，但可以设置单个线程栈的大小；而系统的用户空间一共是3G，除了Text/Data/BSS /MemoryMapping几个段之外，Heap和Stack空间的总量有限，是此消彼长的。因此遇到这个错误，可以通过两个途径解决： 

* 通过 -Xss启动参数减少单个线程栈大小，这样便能开更多线程（当然不能太小，太小会出现StackOverflowError）； 
* 通过-Xms -Xmx 两参数减少Heap大小，将内存让给Stack（前提是保证Heap空间够用）。 

#### java.lang.StackOverflowError 
这也内存溢出错误的一种，即线程栈的溢出，要么是方法调用层次过多（比如存在无限递归调用），要么是线程栈太小。 

优化程序设计，减少方法调用层次；调整 -Xss 参数增加线程栈大小。


# PS

> 本文部分内容来源于网络，无法找到出处，原作者可联系处理

