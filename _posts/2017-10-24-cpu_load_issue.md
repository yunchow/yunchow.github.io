---
layout: post
title: CPU 负载过高问题排查
categories: [技术]
description: CPU 负载过高问题排查
keywords: JVM, GC, Java
---

这几天一直收到线上的机器报 CPU load 过高的短信，然后抽时间排查了一下原因，分析过程如下

> uptime
> top

先看一下所有的进程情况，然后输入 「shift + P」 按 cpu 占用情况排序，按「1」可以显示 CPU 所有的内核占用情况，%CPU 这个值会超过 100% 的原因也是因为把的所有的内核的占比加起来所致，从而找到最大的进程号。

> top -H -p pid

根据进程号显示出来所有的线程，同理找到占用 CPU 负载高的线程号，得到如下输出

```
  PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
10686 admin     20   0 11.2g 8.8g 8124 R 79.6 18.7  5125602h java
10683 admin     20   0 11.2g 8.8g 8124 R 73.7 18.7  5125604h java
10687 admin     20   0 11.2g 8.8g 8124 R 70.3 18.7  5125605h java
10685 admin     20   0 11.2g 8.8g 8124 S 46.4 18.7  5125602h java
10684 admin     20   0 11.2g 8.8g 8124 S 36.2 18.7  5125602h java
10689 admin     20   0 11.2g 8.8g 8124 S  0.7 18.7   1053:45 java
10671 admin     20   0 11.2g 8.8g 8124 S  0.3 18.7   1618:35 java
10677 admin     20   0 11.2g 8.8g 8124 S  0.3 18.7   1620:00 java
10681 admin     20   0 11.2g 8.8g 8124 S  0.3 18.7   1617:33 java
10688 admin     20   0 11.2g 8.8g 8124 S  0.3 18.7  53698:55 java
10762 admin     20   0 11.2g 8.8g 8124 S  0.3 18.7 118:48.34 java
13260 admin     20   0 11.2g 8.8g 8124 S  0.3 18.7   9484:44 java
15623 admin     20   0 11.2g 8.8g 8124 S  0.3 18.7 148:37.21 java
```

现在拿到了最占 CPU 的线程号，再将线程号转换成 16 进制

> printf "%x\n" pid

得到如下结果
```
29be
29bb
29bf
```

然后用 jstack 将进程的 Thread dump 一份出来

> jstack -l pid > ~/jstack-10-23.log

然后用上面得到的 16 进制的线程号到 jstack-10-23.log 定位的负载最高的线程，会得到如下结果
```
"Gang worker#12 (Parallel GC Threads)" os_prio=0 tid=0x00007f234610b000 nid=0x29b5 runnable

"Gang worker#13 (Parallel GC Threads)" os_prio=0 tid=0x00007f234610c000 nid=0x29b6 runnable

"Gang worker#14 (Parallel GC Threads)" os_prio=0 tid=0x00007f234610c800 nid=0x29b7 runnable

"Gang worker#15 (Parallel GC Threads)" os_prio=0 tid=0x00007f234610d800 nid=0x29b8 runnable

"Gang worker#16 (Parallel GC Threads)" os_prio=0 tid=0x00007f2346118000 nid=0x29b9 runnable

"Gang worker#17 (Parallel GC Threads)" os_prio=0 tid=0x00007f2346119000 nid=0x29ba runnable

"Concurrent Mark-Sweep GC Thread" os_prio=0 tid=0x00007f234611d800 nid=0x29c0 runnable

"Gang worker#0 (Parallel CMS Threads)" os_prio=0 tid=0x00007f2346119800 nid=0x29bb runnable

"Gang worker#1 (Parallel CMS Threads)" os_prio=0 tid=0x00007f234611a800 nid=0x29bc runnable

"Gang worker#2 (Parallel CMS Threads)" os_prio=0 tid=0x00007f234611b000 nid=0x29bd runnable

"Gang worker#3 (Parallel CMS Threads)" os_prio=0 tid=0x00007f234611c000 nid=0x29be runnable

"Gang worker#4 (Parallel CMS Threads)" os_prio=0 tid=0x00007f234611c800 nid=0x29bf runnable
```

到这一步已经非常明显了，原来是 JVM GC 线程占用太多的资源，再去看一下 GC 线程为什么占用这么高的资源

这是一段 GC 日志

```
2017-10-23T11:01:41.827+0800: 16057248.188: [GC (CMS Initial Mark) [1 CMS-initial-mark: 6437341K(7340032K)] 6446647K(8257536K), 0.0133200 secs] [Times: user=0.06 sys=0.01, real=
0.01 secs]
2017-10-23T11:01:41.842+0800: 16057248.203: [CMS-concurrent-mark-start]
2017-10-23T11:02:13.573+0800: 16057279.933: [CMS-concurrent-mark: 31.692/31.730 secs] [Times: user=104.57 sys=12.58, real=31.73 secs]
2017-10-23T11:02:13.573+0800: 16057279.934: [CMS-concurrent-preclean-start]
2017-10-23T11:02:13.666+0800: 16057280.027: [CMS-concurrent-preclean: 0.093/0.093 secs] [Times: user=0.09 sys=0.00, real=0.09 secs]
2017-10-23T11:02:13.667+0800: 16057280.028: [CMS-concurrent-abortable-preclean-start]
 CMS: abort preclean due to time 2017-10-23T11:02:18.699+0800: 16057285.059: [CMS-concurrent-abortable-preclean: 1.626/5.032 secs] [Times: user=1.67 sys=0.02, real=5.03 secs]
2017-10-23T11:02:18.731+0800: 16057285.091: [GC (CMS Final Remark) [YG occupancy: 9478 K (917504 K)]2017-10-23T11:02:18.731+0800: 16057285.092: [GC (CMS Final Remark) 2017-10-23
T11:02:18.732+0800: 16057285.092: [ParNew: 9478K->30K(917504K), 0.1088128 secs] 6446820K->6437371K(8257536K), 0.1094402 secs] [Times: user=1.74 sys=0.01, real=0.11 secs]
2017-10-23T11:02:18.841+0800: 16057285.201: [Rescan (parallel) , 0.0540765 secs]2017-10-23T11:02:18.895+0800: 16057285.255: [weak refs processing, 0.0001402 secs]2017-10-23T11:0
2:18.895+0800: 16057285.256: [class unloading, 0.1075606 secs]2017-10-23T11:02:19.003+0800: 16057285.364: [scrub symbol table, 0.0269038 secs]2017-10-23T11:02:19.030+0800: 16057
285.390: [scrub string table, 0.0047259 secs][1 CMS-remark: 6437341K(7340032K)] 6437371K(8257536K), 0.3041883 secs] [Times: user=2.77 sys=0.02, real=0.31 secs]
2017-10-23T11:02:19.036+0800: 16057285.397: [CMS-concurrent-sweep-start]
2017-10-23T11:02:31.873+0800: 16057298.234: [CMS-concurrent-sweep: 12.836/12.836 secs] [Times: user=12.92 sys=0.03, real=12.83 secs]
2017-10-23T11:02:31.874+0800: 16057298.234: [CMS-concurrent-reset-start]
2017-10-23T11:02:32.207+0800: 16057298.568: [CMS-concurrent-reset: 0.333/0.334 secs] [Times: user=0.34 sys=0.00, real=0.34 secs]
```

原来是 CMS 线程一直在执行垃圾回收，再看一下内存 Heap 的情况

> jmap -heap pid

根据输出可以看出来，原来是因为内存占用太多，一直触发老年代的 CMS GC，从而占用大量 CPU 资源。

找到问题后，就好解决了。重启电脑，让内存释放即可。但是这里存在一个延伸的问题，为什么老年代的内存占用会走过 CMS 设计的垃圾回收的阀值（-XX:CMSInitiatingOccupancyFraction=percent），原因是报警的机器是用来搜索用的，里面缓存了大量的文件索引，所以超过了这个值。

从这个案例可以看出来，在设计一个缓存策略的时候，控制缓存的大小，及时删除过期缓存，防止内存的溢出是非常重要的。因为缓存的设计是比较复杂的，这里不多说，计划再另写一篇文章详细介绍缓存的设计技巧。


