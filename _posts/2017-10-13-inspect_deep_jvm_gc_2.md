---
layout: post
title: 深入理解 JVM 垃圾回收原理（下）
categories: [技术]
description: 深入理解 JVM 垃圾回收原理（下）
keywords: JVM, GC, Java
---

> 接上篇
>
> 由于文章太长，分两上、下两篇文章来分别说明，这一篇是下篇

## 垃圾回收器

### 串行回收器
如果使用串行回收器，那么年轻代和老年代的垃圾回收都只能使用到一个 CPU 的一个内核去执行，并且是独占的，也就是 **Stop-the-world**，所有的应用线程是挂起状态。

#### 年轻代使用串行回收器
下图描述了年轻代的串行回收过程。在 Eden 区的存活对象会被拷贝到一个空的存活区『Survivor』，在这个时候，如果发现较大的对象超过了存活区的空间，那么会直接拷贝到老年代。每一次的对象拷贝的时候，会把对象的年龄『Age』增加一次，当这个对象的年龄超过一定的阀值会被直接拷贝到老年代，否则拷贝进存活区等待下一次的「Copy Collect」。注意这里还有另一个例外的情况，就是如果目标存活区的空间不足以容纳 Eden 区和 From 区的对象时，那么这些对象会直接进入老年代。由此可以看出来年轻代每一块空间大小的设置是需要做针对性优化的。
![serial_ygc.png](/imgs/serial_ygc.png)

当年轻代的回收结束后，Eden 区和其中一个存活区会是空的，而另一个存活区里的对象全是存活的对象。
![ygc_after.png](/imgs/ygc_after.png)

#### 老年代使用串行回收器
标记-清理-压缩（mark-sweep-compact）回收算法是老年代和持久代使用的串行回收算法。在标记阶段回收器会对所有存活的对象打上一个标记，然后在清理阶段根据标记去清理掉那些垃圾对象，然后再执行压缩，将所有的仍然存活的对象移动到一个连续的内存空间内，去掉内存中的空间碎片。
![old_ser_gc.png](/imgs/old_ser_gc.png)

#### 什么时候使用串行回收器
对于大多数运行在个人客户端上的应用推荐使用串行回收，因为这种情况对低延时没有太大的要求，尤其是当今的硬件水平，对于一个 64Mb 大小的 Heap 空间执行一次 Full GC 可以在半秒内完成。

> 但是对于服务端的互联网应用，对于 JVM 的延时是不可接受的，那么种情况是不推荐使用串行回收的。

#### 串行回收配置参数
```
-XX:+UseSerialGC
```
在 JDK5 的非 server 模式下默认开启串行回收。

## 并行回收器
现在很多的 Java 应用运行的机器都会有很大的物理内存和多核 CPU，那么并行回收器（也叫吞吐量回收器）可以最大限度的去使用这些物理资源，从而大大的降低回收延时。

#### 年轻代使用并行回收器
对年轻代的串行收集器进行优化后，让其能够使用多核 CPU 的优势执行并行回收。但他依然是一个**停机复制回收器**（stop-the-world and copying collector）。因为他能够使用多 CPU，从而大大降低延时并提高系统吞量。
![par_new_gc.png](/imgs/par_new_gc.png)

#### 老年代使用并行回收器
与老年代使用串行回收器的处理过程是相同的，需要标记、清理然后再压缩。

#### 什么时候使用并行回收器
在多核 CPU 的机器上，并行回收能大大受益，大大的降低延时同时提高吞吐量。缺点就是老年代的回收代价依然巨大。
> 在大多情况下，优先采用并行回收器而非串行回收

#### 并行回收器配置参数
```
-XX:+UseParallelGC
```
在 JDK5 的服务器模式下，默认开始并行回收器。

## 并行压缩回收器
并行压缩回收器是在 JDK5 update 6 这个版本后引入的，他是在并行回收基础上对老年代的回收进行了优化，最终是用来替代并行回收器的。

#### 年轻代使用并行压缩回收器
与年轻代使用并行回收情况相同。

#### 老年代使用并行压缩回收器
老年代和持久代在收集的时候都是停机状态。回收器会分成三阶段（标记、回收、压缩），每代的内存空间都会被划分若干成固定大小的区域。
首先在标记阶段，基于划分的区域系统会启动基于标记线程，对这些区域进行并行的标记，当发现这些区域中存在至少一个存活对象时，就对这个区域进行登记，记录区域的位置和大小信息。
在回收阶段就是基于标记的区域信息，对区域进行回收。对于这些区域实际上会有一个密度的区别，对于密度小的区域，可以将存活动的对象转移到别的区域，然后再回收掉这个区域，而对于密度大的区域，可以先不管。
在压缩阶段也就是去调整这些区域的密度问题。

#### 什么时候使用并行压缩回收
对于多核 CPU 的机器建议使用这种回收器。并且根据 CPU 的内核数量优化并行回收的线程数
```
-XX:ParallelGCThreads=n
```

#### 配置参数
```
-XX:+UseParallelOldGC
```

## CMS 回收器
对另外一些大多数应用，端到端的吞量并不像快速响应用户请求那么重要。年轻代的回收不会引起太久的应用挂起现象，但是老年代的回收因为不是很频繁，所以会引直太大的挂机现象，尤其是在堆内存很大的时候，应用挂起的现象更加的严重。为了解决这个问题，JVM 引入了 CMS(Concurrent Mark Sweep) 回收器，也叫做 **低延时回收器**

#### 年轻代使用 CMS

年轻代使用 CMS 和使用并行回收情况一致，采用的是并行的复制回收算法，**注意还是会有应用短暂挂起的现象**。

#### 老年代使用 CMS
CMS 在老年代的大部分回收过程都是与应用线程同时执行的，概要过程如下图所示
![cms_process.png](/imgs/cms_process.png)

CMS 的回收过程会经过几个过程

* 初始标记 Initial mark：单线程运行去标记那些从 GC Root 可直达的存活对象，**应用被挂起**。
* 并行标记阶段 Concurent marking phase：从上一个阶段标记的对象开始，并行去标记其他的存活对象。
* 预回收 Preclean：回收掉已经被标记出来的垃圾，从而释放空间。
* 重新标记 Remark：因为上一阶段是并行的，在并行标记的时候可能还是会有垃圾产生，所以**需要将应用挂起**再重新标记一次，但是进行重新标记的时候，只会去扫描那些刚刚发生变化的对象。因为重新标记相对初始标记工作量大出许多，所以这个阶段是多线程运行的，从而提高标记效率。
* 并行回收 Concurrent sweep phase：堆中所有的垃圾已经被标记出来了，所以这个阶段会去并行的回收掉这些垃圾从而释放空间。
* 重置状态 Reset：释放掉 GC 缓存，准备下一次回收。

如下图，对比了串行回收和 CMS 回收的区别
![cms_vs_serial.png](/imgs/cms_vs_serial.png)

重新标记虽然增加了 GC 的处理复杂度，但是这样做却减少了应用的暂停时间。

CMS 回收器是惟一的不会去压缩碎片空间的回收器，所以他会产生大量的空间碎片，如下图
![cms_compact.png](/imgs/cms_compact.png)

这样做会节约时间，但是会导致内存空间不连续。
另一个缺点就是 CMS 会要求更大的堆空间。
不像别的回收器，CMS 并不是在老年代的空间满的时候才开始回收，他会提前进行回收，从而避免堆空间满的情况，可以通过参数配置回收水位的百分比。
```
-XX:CMSInitialtingOccupancyFraction=n
```
n 是老年代大小的百分比，默认是 68。

#### CMS 回收配置参数
通过下面的参数显示指定使用 CMS 回收器。
```
-XX:+UseConcMarkSweepGC
```
打开增量模式
```
-XX:+CMSIncrementalMode
```

## OutOfMemoryError
当 JVM 内存空间不足时会抛出这个异常，具体的原因分成如下几种

#### Java heap space
这种情况是堆空间不够导致，可通过下面参数调整
```
-Xmx4096Mb
```

#### PermGen space
这是原因持久代的空间不足所致，通过这个参数设置
```
-XX:MaxPermSize=n
```
n 就是要设置的大小，如 128Mb

#### Requested array size exceeds VM limit
这是因数申请的数组大小超过了堆空间所致，原因是堆的空间太小，调大即可。

## 垃圾回收调优工具
#### 打印 GC 明细
对线上系统开启 GC 明细日志
```
-XX:+PrintGCDetails
```

#### 打印 GC 时间
```
-XX:+PrintGCTimeStamps
```
#### jmap
把堆内存导出后，利用三方工具进行离线分析，如 MAT、ViualVM
```
jmap -dump:format=bin,file=/tmp/heap.dump pid
```

#### jstat
实时查看 GC 情况
```
jstat -gcutil pid 1000
```

## GC 重要的配置参数
#### 垃圾回收器的选择
|参数|垃圾回收器|
|-------|--------|
|–XX:+UseSerialGC|串行回收器|
|–XX:+UseParallelGC|并行回收器|
|–XX:+UseParallelOldGC|并行压缩回收器|
|–XX:+UseConcMarkSweepGC|CMS 回收器|

#### 垃圾回收器统计
参数|描述
|-------|--------|
|–XX:+PrintGC|打印每一次的回收基本信息|
|–XX:+PrintGCDetails|打印每一次回收的详细信息|
|–XX:+PrintGCTimeStamps|打印垃圾回收的时间信息|
|-Xloggc:/tmp/gc.log|保存 GC 日志到文件|

#### 堆大小参数
|参数|默认值|描述|
|---|---|---|
|–Xmsn||堆的初始大小，单位字节|
|–Xmxn||堆的最大大小，单位是字节|
|–XX:MinHeapFreeRatio=minimum|40|
|–XX:MaxHeapFreeRatio=maximum|70|
|–XX:NewSize=n|平台独立|年轻代默认初始大小，单位字节|
|–XX:NewRatio=n|客户端模式为 2，服务端模式为 8|老年代和年轻代的比值|
|–XX:SurvivorRatio=n|32|Eden 与存活区的比值|
|–XX:MaxPermSize=n|平台独立|持久代最大大小|

#### 并行回收器参数
|参数|默认值|描述|
|---|---|---|
|–XX:ParallelGCThreads=n|CPU 的数量|并行回收的线程数|
|–XX:MaxGCPauseMillis=n|无|期望的暂停的时间，单位毫秒|
|–XX:GCTimeRatio=n|99|设置全局总共花在垃圾回收上的时间，公式 1/(1+n)|

#### CMS 参数
|参数|默认值|描述|
|---|---|---|
|–XX:+CMSIncrementalMode|未开启|开启增量模式|
|–XX:+CMSIncrementalPacing|未开启|
|–XX:ParallelGCThreads=n|CPU 数量|年轻代和老年代的并发线程数|

## 实例举例
```
CATALINA_OPTS="-server"
CATALINA_OPTS="${CATALINA_OPTS} -Xms4g -Xmx4g"
CATALINA_OPTS="${CATALINA_OPTS} -XX:PermSize=256m -XX:MaxPermSize=256m"
CATALINA_OPTS="${CATALINA_OPTS} -XX:MetaspaceSize=512m -XX:MaxMetaspaceSize=512m"
CATALINA_OPTS="${CATALINA_OPTS} -Xmn2g"
CATALINA_OPTS="${CATALINA_OPTS} -XX:MaxDirectMemorySize=1g"
CATALINA_OPTS="${CATALINA_OPTS} -XX:SurvivorRatio=10"
CATALINA_OPTS="${CATALINA_OPTS} -XX:+UseConcMarkSweepGC -XX:+UseCMSCompactAtFullCollection -XX:CMSMaxAbortablePrecleanTime=5000"
CATALINA_OPTS="${CATALINA_OPTS} -XX:+CMSClassUnloadingEnabled -XX:CMSInitiatingOccupancyFraction=80 -XX:+UseCMSInitiatingOccupancyOnly"
CATALINA_OPTS="${CATALINA_OPTS} -XX:+ExplicitGCInvokesConcurrent -Dsun.rmi.dgc.server.gcInterval=2592000000 -Dsun.rmi.dgc.client.gcInterval=2592000000"
CATALINA_OPTS="${CATALINA_OPTS} -XX:ParallelGCThreads=${CPU_COUNT}"
CATALINA_OPTS="${CATALINA_OPTS} -Xloggc:${MIDDLEWARE_LOGS}/gc.log -XX:+PrintGCDetails -XX:+PrintGCDateStamps"
CATALINA_OPTS="${CATALINA_OPTS} -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=${MIDDLEWARE_LOGS}/java.hprof"

```


