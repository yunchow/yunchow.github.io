---
layout: post
title: 一次 OOM 引起的 Full GC 排查过程记录
categories: [技术]
description: 一次 OOM 引起的 Full GC 排查过程记录
keywords: FGC, java, gc, full gc, oom
---

> 国庆 8 天假在高速上堵了4天，对安徽的高速无力吐曹。
> 
> 今天到公司第一件事就是解决假期遗留的问题。我们系统在3号和6号有2次停机事故，现在就是需要来找到系统挂机的根本原因。
> 
> 眼看只有1个月就要双11了，不找到问题原因是根本睡不好觉了。

## CMS 垃圾回收算法

现在国内的互联网企业基本都是用的 CMS 回收算法，这里简单介绍一下，想要了解的更多更深入，可以参考这篇管方的 [Memory Management White Paper](http://www.oracle.com/technetwork/java/javase/tech/memorymanagement-whitepaper-1-150020.pdf)

![CMS_GC_Process.png](/imgs/cms_process.png)

一个典型的 JVM 参数设置如下：
```java
 -XX:+CMSClassUnloadingEnabled -XX:CMSInitiatingOccupancyFraction=80 -XX:CMSMaxAbortablePrecleanTime=5000 -XX:+ExplicitGCInvokesConcurrent -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/home/admin/logs/java.hprof -XX:InitialHeapSize=4294967296 -XX:+ManagementServer -XX:MaxDirectMemorySize=1073741824 -XX:MaxHeapSize=4294967296 -XX:MaxMetaspaceSize=536870912 -XX:MaxNewSize=2147483648 -XX:MetaspaceSize=536870912 -XX:NewSize=2147483648 -XX:OldPLABSize=16 -XX:ParallelGCThreads=4 -XX:+PrintGC -XX:+PrintGCDateStamps -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -XX:SurvivorRatio=10 -XX:+UseCMSCompactAtFullCollection -XX:+UseCMSInitiatingOccupancyOnly -XX:+UseCompressedClassPointers -XX:+UseCompressedOops -XX:+UseConcMarkSweepGC -XX:+UseParNewGC
```

如果使用的是 Tomcat 容器，可以直接在 setenv.sh 文件中这样设置
```java
# tomcat jvm options
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
CATALINA_OPTS="${CATALINA_OPTS} -Djava.awt.headless=true"
CATALINA_OPTS="${CATALINA_OPTS} -Dsun.net.client.defaultConnectTimeout=10000"
CATALINA_OPTS="${CATALINA_OPTS} -Dsun.net.client.defaultReadTimeout=30000"
CATALINA_OPTS="${CATALINA_OPTS} -DJM.LOG.PATH=${MIDDLEWARE_LOGS}"
CATALINA_OPTS="${CATALINA_OPTS} -DJM.SNAPSHOT.PATH=${MIDDLEWARE_SNAPSHOTS}"
CATALINA_OPTS="${CATALINA_OPTS} -Dfile.encoding=${JAVA_FILE_ENCODING}"
CATALINA_OPTS="${CATALINA_OPTS} -Dhsf.publish.delayed=true"
CATALINA_OPTS="${CATALINA_OPTS} -Dcom.sun.management.jmxremote.port=10089 -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false"
```

## GC 日志

先看一段正常的 GC 日志 [^1]

```java
2017-10-02T23:00:02.311+0800: 11.345: [GC 11.345: [ParNew: 892042K->46565K(961216K), 0.0732490 secs] 892042K->46565K(2009792K), 0.0733230 secs] [Times: user=0.11 sys=0.02, real=0.07 sec
s]
2017-10-02T23:00:03.626+0800: 12.659: [GC [1 CMS-initial-mark: 0K(1048576K)] 123379K(2009792K), 0.0348150 secs] [Times: user=0.05 sys=0.00, real=0.04 secs]
2017-10-02T23:00:03.661+0800: 12.694: [CMS-concurrent-mark-start]
2017-10-02T23:00:03.849+0800: 12.882: [CMS-concurrent-mark: 0.179/0.188 secs] [Times: user=0.50 sys=0.01, real=0.18 secs]
2017-10-02T23:00:03.849+0800: 12.882: [CMS-concurrent-preclean-start]
2017-10-02T23:00:03.857+0800: 12.891: [CMS-concurrent-preclean: 0.008/0.009 secs] [Times: user=0.02 sys=0.00, real=0.01 secs]
2017-10-02T23:00:03.857+0800: 12.891: [CMS-concurrent-abortable-preclean-start]
2017-10-02T23:00:08.061+0800: 17.094: [CMS-concurrent-abortable-preclean: 3.744/4.203 secs] [Times: user=7.44 sys=0.33, real=4.21 secs]
2017-10-02T23:00:08.061+0800: 17.095: [GC[YG occupancy: 497553 K (961216 K)]17.095: [Rescan (parallel) , 0.0653640 secs]17.160: [weak refs processing, 0.0000080 secs]17.160: [class unlo
ading, 0.0160290 secs]17.176: [scrub symbol & string tables, 0.0161840 secs] [1 CMS-remark: 0K(1048576K)] 497553K(2009792K), 0.0985590 secs] [Times: user=0.20 sys=0.00, real=0.09 secs]
2017-10-02T23:00:08.161+0800: 17.195: [CMS-concurrent-sweep-start]
2017-10-02T23:00:08.224+0800: 17.258: [CMS-concurrent-sweep: 0.050/0.063 secs] [Times: user=0.15 sys=0.00, real=0.06 secs]
2017-10-02T23:00:08.225+0800: 17.259: [CMS-concurrent-reset-start]
2017-10-02T23:00:08.266+0800: 17.300: [CMS-concurrent-reset: 0.038/0.042 secs] [Times: user=0.09 sys=0.02, real=0.04 secs]
2017-10-02T23:00:25.728+0800: 34.762: [GC 34.762: [ParNew: 920421K->55295K(961216K), 0.0687000 secs] 920421K->55295K(2009792K), 0.0688190 secs] [Times: user=0.14 sys=0.02, real=0.07 sec
s]
```

再来看一段 Full GC 日志 [^1]

```
2017-10-03T20:50:26.642+0800: 78460.869: [GC [1 CMS-initial-mark: 1048576K(1048576K)] 1559116K(2009792K), 0.2449110 secs] [Times: user=0.23 sys=0.00, real=0.25 secs]
2017-10-03T20:50:26.887+0800: 78461.114: [CMS-concurrent-mark-start]
2017-10-03T20:50:27.375+0800: 78461.603: [Full GC 78461.603: [CMS2017-10-03T20:50:29.495+0800: 78463.722: [CMS-concurrent-mark: 2.568/2.608 secs] [Times: user=3.60 sys=0.07, real=2.61 s
ecs]
 (concurrent mode failure): 1048576K->1048574K(1048576K), 5.3147180 secs] 2009791K->1155619K(2009792K), [CMS Perm : 225247K->225198K(375440K)], 5.3150620 secs] [Times: user=5.18 sys=0.0
3, real=5.31 secs]
2017-10-03T20:50:32.781+0800: 78467.009: [GC [1 CMS-initial-mark: 1048574K(1048576K)] 1258404K(2009792K), 0.1375430 secs] [Times: user=0.13 sys=0.00, real=0.14 secs]
2017-10-03T20:50:32.921+0800: 78467.148: [CMS-concurrent-mark-start]
2017-10-03T20:50:33.575+0800: 78467.803: [Full GC 78467.803: [CMS2017-10-03T20:50:35.621+0800: 78469.848: [CMS-concurrent-mark: 2.680/2.700 secs] [Times: user=3.58 sys=0.08, real=2.70 s
ecs]
 (concurrent mode failure): 1048575K->1048575K(1048576K), 5.5099670 secs] 2009791K->1163408K(2009792K), [CMS Perm : 225244K->225198K(375440K)], 5.5103230 secs] [Times: user=5.36 sys=0.0
2, real=5.51 secs]
2017-10-03T20:50:39.117+0800: 78473.345: [GC [1 CMS-initial-mark: 1048575K(1048576K)] 1249142K(2009792K), 0.1157570 secs] [Times: user=0.11 sys=0.00, real=0.11 secs]
2017-10-03T20:50:39.247+0800: 78473.474: [CMS-concurrent-mark-start]
2017-10-03T20:50:39.901+0800: 78474.128: [Full GC 78474.129: [CMS2017-10-03T20:50:41.873+0800: 78476.101: [CMS-concurrent-mark: 2.596/2.627 secs] [Times: user=3.75 sys=0.16, real=2.62 s
ecs]
 (concurrent mode failure): 1048575K->1048575K(1048576K), 5.4820890 secs] 2009791K->1171652K(2009792K), [CMS Perm : 225240K->225200K(375440K)], 5.4824540 secs] [Times: user=5.35 sys=0.0
2, real=5.48 secs]
2017-10-03T20:50:45.626+0800: 78479.853: [GC [1 CMS-initial-mark: 1048575K(1048576K)] 1630564K(2009792K), 0.3244190 secs] [Times: user=0.31 sys=0.00, real=0.32 secs]
2017-10-03T20:50:45.951+0800: 78480.178: [CMS-concurrent-mark-start]
2017-10-03T20:50:46.359+0800: 78480.586: [Full GC 78480.586: [CMS2017-10-03T20:50:48.445+0800: 78482.673: [CMS-concurrent-mark: 2.479/2.494 secs] [Times: user=3.33 sys=0.04, real=2.49 s
ecs]
```

关于 GC 日志的详解参考这篇文章：[Understanding CMS GC Logs](https://blogs.oracle.com/poonam/understanding-cms-gc-logs)


## 原因分析

JVM 频繁的 GC 说明应用的内存不够用了，因为 JVM 启动 GC 也是需要内存的，但是内存是因为什么原因被耗尽，这需要排查系统的日志，最后排查的结果是因为线上的 RPC 调用导致大量的长连接超时，占用了大量的内存，接近 2G，RPC 超时的原因是服务端在进行压测引起的。但是应用因为 Docker 化的原因，默认配置的内存只有 2G，从而导致了频繁的触发 Full GC。解决的办法有如下 2 点：

1. 将 RPC 调用的超时时间调短，但是种方案有一个缺点，就是会导致大量的业务失败，而业务主流程的失败如果不太能接受，这种方案就不适用
2. 将线上的应用堆内存调成 4G

综合考虑各方面因素选择方案 2。

## 分析工具

在遇到 OOM 的时候，我们需要借助一些工具去分析 Heap， 常见的比如 

* [MAT](https://www.eclipse.org/mat/)
* [Jprofiler](https://www.ej-technologies.com/products/jprofiler/overview.html)
* VisualVM

当然还有 JDK 自带的工具：

* jps 查看 JVM 进程
* jstat 实时监测 JVM
* jmap 用来分析 Heap 对象的内存占用情况，排查 OOM 的利器
* jhat 对 dump 出来的堆文件进行在线分析，可以用 MAT 代替
* jstack 分析 Thread 运行情况，分析死锁
* jinfo 查看 JVM 进程的配置信息

详细的用法可以查看帮助，或者查看这 3 篇文章

* [JVM性能调优监控工具jps、jstack、jmap、jhat、jstat、hprof使用详解](http://blog.csdn.net/dragonassassin/article/details/51010947)
* [JVM性能调优监控工具jps、jstack、jmap、jhat、jstat、hprof使用详解](https://my.oschina.net/feichexia/blog/196575)
* [内存分析工具 MAT 的使用](http://blog.csdn.net/aaa2832/article/details/19419679)

## Linux 分析

* top 查看系统整体性能情况
* free -m 查看内存占用
* uptime 查看 CPU 负载情况
* iostat

## 参考

[^1]: [CMS GC 日志](http://www.cnblogs.com/kakaxisir/p/6209266.html)




