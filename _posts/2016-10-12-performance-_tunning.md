---
layout: post
title: 我怎么做 Java 应用性能优化
categories: [技术]
description: 我怎么做 Java 应用性能优化
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

### jps -h
查看帮助信息

## jmap
jmap 用于查看 JVM 堆内存的情况，包括了堆内存汇总、对象大小、类加载、永久代（方法区，常量池）等相关信息，jmap 还可以 dump 堆快照到一个二进制文件进行离线分析。

### jmap -heap pid
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

### jmap -histo pid
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

### jmap -permstat pid
用来搜集持久代的统计信息，执行时间很慢

### jmap -dump pid
```
jmap -dump:format=b,file=heap.bin pid
```
导出堆内存的快照，便于对内存进行离线分析，但是需要借助一些第三方的工作，比如：MAT, Jprofiler

* 堆内存概要汇总信息

![堆内存概要汇总信息](/imgs/heap_sum.png)

* 堆内存详细信息

![堆内存详细信息](/imgs/heap_histo.png)

这里有 2 个概念简单解释一下

* Shallow Heap 对象本身占用的内存大小，对象引用的别的对象不包含在内
* Retained Heap 对象被回收可以释放掉的内存大小，包括了自身的大小和它引用的可释放的空间大小

## jhat
对导出的 Java 堆文件进行在线的分析，功能类似 MAT

```
jhat -J-mx512m -port 7000 <file>
```
这样开启服务后，可以在本地通过浏览器进行访问

## jstat
查看 JVM 运行时的一些信息，可通过 -options 查看所有功能，如下

```
./jstat -options
-class
-compiler
-gc
-gccapacity
-gccause
-gcnew
-gcnewcapacity
-gcold
-gcoldcapacity
-gcpermcapacity
-gcutil
-printcompilation
```

### jstat -gcutil pid 1000

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

查看线程相关的信息，常用来分析死锁

```
./jstack -h
Usage:
    jstack [-l] <pid>
        (to connect to running process)
    jstack -F [-m] [-l] <pid>
        (to connect to a hung process)
    jstack [-m] [-l] <executable> <core>
        (to connect to a core file)
    jstack [-m] [-l] [server_id@]<remote server IP or hostname>
        (to connect to a remote debug server)

Options:
    -F  to force a thread dump. Use when jstack <pid> does not respond (process is hung)
    -m  to print both java and native frames (mixed mode)
    -l  long listing. Prints additional information about locks
    -h or -help to print this help message
```

## jinfo
打印系统配置信息，如下

```
VM Flags:

-Djava.util.logging.config.file=/home/admin/ma/.default/conf/logging.properties -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Dcatalina.vendor=alibaba -Djava.securi
ty.egd=file:/dev/./urandom -Dlog4j.defaultInitOverride=true -Dorg.apache.tomcat.util.http.ServerCookie.ALLOW_EQUALS_IN_VALUE=true -Dorg.apache.tomcat.util.http.ServerCookie.ALLOW_HTTP_S
EPARATORS_IN_V0=true -Xms4g -Xmx4g -XX:PermSize=96m -XX:MaxPermSize=384m -Xmn2g -XX:+UseConcMarkSweepGC -XX:+UseCMSCompactAtFullCollection -XX:CMSMaxAbortablePrecleanTime=5000 -XX:+CMSC
lassUnloadingEnabled -XX:+UseCMSInitiatingOccupancyOnly -XX:CMSInitiatingOccupancyFraction=80 -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/home/admin/logs/java.hprof -Xloggc:/home/
admin/logs/gc.log -XX:+PrintGCDetails -XX:+PrintGCDateStamps -Djava.awt.headless=true -Dsun.net.client.defaultConnectTimeout=10000 -Dsun.net.client.defaultReadTimeout=30000 -XX:MaxDirec
tMemorySize=1g -XX:SurvivorRatio=10 -XX:+ExplicitGCInvokesConcurrent -Dsun.rmi.dgc.server.gcInterval=2592000000 -Dsun.rmi.dgc.client.gcInterval=2592000000 -XX:ParallelGCThreads=4 -DJM.L
OG.PATH=/home/admin/logs -Dfile.encoding=GB18030 -Dhsf.publish.delayed=true -Dproject.name=ma -Djava.endorsed.dirs=/opt/taobao/tomcat/endorsed -Dcatalina.logs=/home/admin/ma/logs/catali
na -Dcatalina.base=/home/admin/ma/.default -Dcatalina.home=/opt/taobao/tomcat -Djava.io.tmpdir=/home/admin/ma/.default/temp
```

# Linux 篇

## vmstat
查看系统内存、IO、CPU信息
```
vmstat 1
procs -----------memory---------- ---swap-- -----io---- --system-- -----cpu------
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      0 2616048      0 1149900    0    0   699 28014    6    0  8  1 87  0  5
 0  0      0 2616124      0 1149904    0    0     0  3540 163918 3226  1  0 93  0  5
 0  0      0 2616312      0 1150044    0    0     0     0 175265 3826  3  1 93  0  3
 0  0      0 2612704      0 1150076    0    0     0     0 203592 3892 15  3 75  0  6
 1  0      0 2612732      0 1150120    0    0     4  4268 190790 3580  4  2 89  0  4
 2  0      0 2612552      0 1150168    0    0     0   164 174234 3658  3  1 92  0  4
 0  0      0 2612244      0 1150204    0    0     0  5144 180386 3511  2  1 93  0  4
 1  0      0 2612224      0 1150224    0    0     4     0 178876 3687  3  2 88  0  7
 0  0      0 2612204      0 1150244    0    0     0  3760 179695 3635  2  0 92  0  6
 0  0      0 2612192      0 1150256    0    0     0  8268 179635 3518  2  1 93  0  4
 1  0      0 2612140      0 1150308    0    0     0   140 183020 3535  3  0 92  0  4
 1  0      0 2612020      0 1150424    0    0     0     0 163945 3867 21  1 72  0  5
```
[详细说明参考这里](http://www.cnblogs.com/zhiqian-ali/p/4930388.html)

## iostat
查看系统 IO 信息
```
$iostat
Linux 2.6.32-358.23.2.ali1172.el5.x86_64 (ma011250055040.eu13)  10/12/2017

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           7.95    0.01    0.90    0.00    4.61   86.52

Device:            tps   Blk_read/s   Blk_wrtn/s   Blk_read   Blk_wrtn
sda             215.36        24.71      2722.61   29741664 3276937160
sda1              0.06         0.00         0.12       2492     144094
sda2            215.24        24.70      2722.09   29733498 3276311048
sda3              0.00         0.00         0.00       2168          2
sda4              0.00         0.00         0.00          8          0
sda5              0.05         0.00         0.40       2066     482016
sdb            3994.04      5627.35    223680.10 6773098528 269222012744
sdb1           3764.68      5627.35    223680.10 6773097376 269222012744
```
[详细说明参考这里](http://www.cnblogs.com/zhiqian-ali/p/4930388.html)

## dmesge
用来查看系统消息，最典型的场景就是发现 Java 进程无帮消失时，查看是否是系统 oom-killer 所为，尤其是在 Docker 环境，因为存在内存超卖的情况，应用无故消失，很有可能是 oom-killer 所为

```
dmesg | grep java
[34341062.176755] [50398]   697 50398  4086406  1956111  60       0             0 java
[34341062.240302] [256082]   698 256082   117782    19857  62       0             0 java
[34341062.257468] Memory cgroup out of memory: Kill process 50398 (java) score 936 or sacrifice child
[34341062.268023] Killed process 50398, UID 697, (java) total-vm:16345624kB, anon-rss:7822592kB, file-rss:1852kB
[34341065.188952] [50416]   697 50398  4086406  1956099  63       0             0 java
[34341065.252456] [256082]   698 256082   117782    19857  62       0             0 java
[34341065.269762] Memory cgroup out of memory: Kill process 50416 (java) score 936 or sacrifice child
[34341065.279322] Killed process 50416, UID 697, (java) total-vm:16345624kB, anon-rss:7822592kB, file-rss:1804kB
```
## free
查看系统内存信息
```
$free -m
             total       used       free     shared    buffers     cached
Mem:          8192       5440       2751          0          0       1107
-/+ buffers/cache:       4333       3858
Swap:         2047          0       2047
```

## uptime
查看系统 CPU 负载情况

```
$uptime
 11:35:50 up 13 days, 22:12,  3 users,  load average: 0.31, 0.41, 0.25
```
## top
查看系统进程信息
```
top - 11:37:11 up 13 days, 22:13,  3 users,  load average: 0.12, 0.33, 0.23
Tasks:  46 total,   1 running,  42 sleeping,   0 stopped,   3 zombie
Cpu(s):  2.1%us,  0.9%sy,  0.0%ni, 92.1%id,  0.0%wa,  0.0%hi,  0.0%si,  4.9%st
Mem:   8388608k total,  5639588k used,  2749020k free,        0k buffers
Swap:  2097144k total,        0k used,  2097144k free,  1137888k cached

   PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
213676 admin     20   0 6502m 4.1g  43m S  9.3 51.0   3:28.50 java
   295 root      20   0  721m  19m 7056 S  0.7  0.2 111:14.49 staragent-core
   297 root      20   0  333m 8872 5632 S  0.3  0.1  44:10.78 staragent-ppf
   410 root      20   0  195m 5428 2792 S  0.3  0.1 159:10.20 alisentry_cli
215495 admin     20   0  202m  66m 1164 S  0.3  0.8   0:00.18 nginx-proxy
     1 root      20   0 10360  724  608 S  0.0  0.0   0:05.74 init
   294 root      20   0 29760 2424 1276 S  0.0  0.0   0:23.86 staragentd
   544 root      20   0 69708 1228  644 S  0.0  0.0   0:07.19 sshd
   554 root      20   0 81908 1316  600 S  0.0  0.0   0:12.92 crond
   618 root      20   0 12044  600  484 S  0.0  0.0   0:00.00 mingetty
   619 root      20   0 16952 1208 1008 S  0.0  0.0   0:00.00 svscanboot
```

## df / du
查看磁盘空间和文件大小，当系统运行一段时间后，日志文件大量堆积有可能会撑爆磁盘，可以通过这个命令查看

```
$df
Filesystem           1K-blocks      Used Available Use% Mounted on
/                     62914560  14369060  48545500  23% /
/dev/v01d             62914560  14369060  48545500  23% /home/admin/cai/alivmcommon
/dev/v02d             62914560  14369060  48545500  23% /home/admin/cai/logs
/dev/v03d             62914560  14369060  48545500  23% /home/admin/cai/top_foot_vm
/dev/v04d             62914560  14369060  48545500  23% /home/admin/localDatas
/dev/v05d             62914560  14369060  48545500  23% /home/admin/logs
/dev/v06d             62914560  14369060  48545500  23% /home/admin/ma/logs
/dev/v07d             62914560  14369060  48545500  23% /home/admin/snapshots/diamond
/dev/v08d             62914560  14369060  48545500  23% /home/admin/tms
/dev/v09d             62914560  14369060  48545500  23% /home/admin/vmcommon
/dev/v10d             62914560  14369060  48545500  23% /home/staragent/plugins
```

```
$du * | head
52  appletviewer
52  apt
0 ControlPanel
52  extcheck
4 HtmlConverter
52  idlj
52  jar
52  jarsigner
52  java
52  javac
```

## ulimit

将系统设置成不限制
```
ulimit -c unlimited
```

查看文件句柄设置
```
$ulimit -a
core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 16384
max locked memory       (kbytes, -l) 64
max memory size         (kbytes, -m) unlimited
open files                      (-n) 65535
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) unlimited
cpu time               (seconds, -t) unlimited
max user processes              (-u) 16384
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited
```

[通过 ulimit 改善系统性能](https4://www.ibm.com/developerworks/cn/linux/l-cn-ulimit/)

# 数据库篇

## Mysql 索引优化
### left-most 最左前缀原则
如果存在联合索引 (A, B, C)，那么查询能够使用索引的情况有这几种

* A, B, C
* A, B
* A

### 执行计划
explain select * from xxx;

```
| id | select_type | table | type | possible_keys | key | key_len | ref | rows | Extra |   
```

> select_type

select查询的类型，主要是区别普通查询和联合查询、子查询之类的复杂查询。

* SIMPLE：查询中不包含子查询或者UNION
* 查询中若包含任何复杂的子部分，最外层查询则被标记为：PRIMARY
* 在SELECT或WHERE列表中包含了子查询，该子查询被标记为：SUBQUERY
* 在FROM列表中包含的子查询被标记为：DERIVED（衍生）
* 若第二个SELECT出现在UNION之后，则被标记为UNION；若UNION包含在 FROM子句的子查询中，外层SELECT将被标记为：DERIVED
* 从UNION表获取结果的SELECT被标记为：UNION RESULT

> type
 
联合查询所使用的类型，表示MySQL在表中找到所需行的方式，又称“访问类型”。

type显示的是访问类型，是较为重要的一个指标，结果值从好到坏依次是：
 system > const > eq_ref > ref > fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index > ALL ，一般来说，得保证查询至少达到range级别，最好能达到ref。

* ALL: 扫描全表
* index: 扫描全部索引树
* range: 扫描部分索引，索引范围扫描，对索引的扫描开始于某一点，返回匹配值域的行，常见于between、<、>等的查询
* ref: 非唯一性索引扫描，返回匹配某个单独值的所有行。常见于使用非唯一索引即唯一索引的非唯一前缀进行的查找
* eq_ref：唯一性索引扫描，对于每个索引键，表中只有一条记录与之匹配。常见于主键或唯一索引扫描
* const, system: 当MySQL对查询某部分进行优化，并转换为一个常量时，使用这些类型访问。如将主键置于where列表中，MySQL就能将该查询转换为一个常量。system是const类型的特例，当查询的表只有一行的情况下， 使用system。
* NULL: MySQL在优化过程中分解语句，执行时甚至不用访问表或索引。

> possible_keys

指出MySQL能使用哪个索引在该表中找到行。查询涉及到的字段上若存在索引，则该索引将被列出，但不一定被查询使用。如果是空的，没有相关的索引。这时要提高性能，可通过检验WHERE子句，看是否引用某些字段，或者检查字段不是适合索引。 

> key

显示MySQL实际决定使用的键。如果没有索引被选择，键是NULL。
 
> key_len

显示MySQL决定使用的键长度。表示索引中使用的字节数，可通过该列计算查询中使用的索引的长度。如果键是NULL，长度就是NULL。文档提示特别注意这个值可以得出一个多重主键里mysql实际使用了哪一部分。
 注：key_len显示的值为索引字段的最大可能长度，并非实际使用长度，即key_len是根据表定义计算而得，不是通过表内检索出的。
 
> ref

显示哪个字段或常数与key一起被使用。
 
> rows

这个数表示mysql要遍历多少数据才能找到，表示MySQL根据表统计信息及索引选用情况，估算的找到所需的记录所需要读取的行数，在innodb上可能是不准确的。

> Extra

包含不适合在其他列中显示但十分重要的额外信息

* Only index，这意味着信息只用索引树中的信息检索出的，这比扫描整个表要快。 
* using where是使用上了where限制，表示MySQL服务器在存储引擎受到记录后进行“后过滤”（Post-filter），如果查询未能使用索引，Using where的作用只是提醒我们MySQL将用where子句来过滤结果集。
* impossible where 表示用不着where，一般就是没查出来啥。 
* Using filesort（MySQL中无法利用索引完成的排序操作称为“文件排序”）当我们试图对一个没有索引的字段进行排序时，就是filesoft。它跟文件没有任何关系，实际上是内部的一个快速排序。 
* Using temporary（表示MySQL需要使用临时表来存储结果集，常见于排序和分组查询），使用filesort和temporary的话会很吃力，WHERE和ORDER BY的索引经常无法兼顾，如果按照WHERE来确定索引，那么在ORDER BY时，就必然会引起Using filesort，这就要看是先过滤再排序划算，还是先排序再过滤划算。


# 中间件篇
## DAL
分布式数据层，对 SQL 执行进行打点，然后统计慢 SQL，方便监控和性能优化

## RPC
服务调用中间件，对服务进行埋点，方便跟踪分布式调用链，便于系统的优化和限流分析

# 其他工具

### shell 循环工具

 工作中常常需要去遍历一个文件，针对每一行记录做一些数据订正或者查询的处理。比如说根据一个包含了订单号列表的文件批量执行快速退款操作。

``` shell
for i in `cat oid.txt`; do curl -d "outerId=$i&bizType=3001" localhost:7001/tool/query/simple3 >> result.txt;echo " " >> result.txt;done
```

# 典型错误

## OOM
OutOfMemoryError 异常除了程序计数器外，虚拟机内存的其他几个运行时区域都有发生 OutOfMemoryError(OOM) 异常的可能

### Java Heap 溢出
一般的异常信息：java.lang.OutOfMemoryError:Java heap spacess

java堆用于存储对象实例，我们只要不断的创建对象，并且保证GC Roots到对象之间有可达路径来避免垃圾回收机制清除这些对象，就会在对象数量达到最大堆容量限制后产生内存溢出异常。
出现这种异常，一般手段是先通过内存映像分析工具(如Eclipse Memory Analyzer)对dump出来的堆转存快照进行分析，重点是确认内存中的对象是否是必要的，先分清是因为内存泄漏(Memory Leak)还是内存溢出(Memory Overflow)。
如果是内存泄漏，可进一步通过工具查看泄漏对象到GC Roots的引用链。于是就能找到泄漏对象时通过怎样的路径与GC Roots相关联并导致垃圾收集器无法自动回收。
如果不存在泄漏，那就应该检查虚拟机的参数(-Xmx与-Xms)的设置是否适当。

### 虚拟机栈和本地方法栈溢出
如果线程请求的栈深度大于虚拟机所允许的最大深度，将抛出StackOverflowError异常。
如果虚拟机在扩展栈时无法申请到足够的内存空间，则抛出OutOfMemoryError异常
这里需要注意当栈的大小越大可分配的线程数就越少。

### 运行时常量池溢出
异常信息：java.lang.OutOfMemoryError:PermGen space

如果要向运行时常量池中添加内容，最简单的做法就是使用String.intern()这个Native方法。该方法的作用是：如果池中已经包含一个等于此String的字符串，则返回代表池中这个字符串的String对象；否则，将此String对象包含的字符串添加到常量池中，并且返回此String对象的引用。由于常量池分配在方法区内，我们可以通过-XX:PermSize和-XX:MaxPermSize限制方法区的大小，从而间接限制其中常量池的容量。

### 方法区溢出
方法区用于存放Class的相关信息，如类名、访问修饰符、常量池、字段描述、方法描述等。

异常信息：java.lang.OutOfMemoryError:PermGen space

方法区溢出也是一种常见的内存溢出异常，一个类如果要被垃圾收集器回收，判定条件是很苛刻的。在经常动态生成大量Class的应用中，要特别注意这点。

## 堆内存溢出
### OutOfMemoryError: Java heap space
这种是java堆内存不够，一个原因是真不够，另一个原因是程序中有死循环；
如果是java堆内存不够的话，可以通过调整JVM下面的配置来解决：

```
-Xms4096M
-Xmx4096M
```

### OutOfMemoryError: GC overhead limit exceeded
JDK6新增错误类型，当GC为释放很小空间占用大量时间时抛出；一般是因为堆太小，导致异常的原因，没有足够的内存。

* 查看系统是否有使用大内存的代码或死循环
* 通过添加JVM配置，来限制使用内存

```
-XX:-UseGCOverheadLimit
```

### OutOfMemoryError: PermGen space
这种是P区内存不够，可通过调整JVM的配置
```
-XX:MaxPermSize=128m
-XXermSize=128m
```

JVM的Perm区主要用于存放Class和Meta信息的,Class在被Loader时就会被放到PermGen space，这个区域成为年老代，GC在主程序运行期间不会对年老区进行清理，默认是64M大小，当程序需要加载的对象比较多时，超过64M就会报这部分内存溢出了，需要加大内存分配，一般 128m 足够。 

### OutOfMemoryError: Direct buffer memory
调整 -XX:MaxDirectMemorySize= 参数，如添加JVM配置
```
-XX:MaxDirectMemorySize=128m
```

### OutOfMemoryError: unable to create new native thread
Stack空间不足以创建额外的线程，要么是创建的线程过多，要么是Stack空间确实小了。 

由于JVM没有提供参数设置总的stack空间大小，但可以设置单个线程栈的大小；而系统的用户空间一共是3G，除了Text/Data/BSS /MemoryMapping几个段之外，Heap和Stack空间的总量有限，是此消彼长的。因此遇到这个错误，可以通过两个途径解决： 

* 通过 -Xss启动参数减少单个线程栈大小，这样便能开更多线程（当然不能太小，太小会出现StackOverflowError）； 
* 通过-Xms -Xmx 两参数减少Heap大小，将内存让给Stack（前提是保证Heap空间够用）。 

### StackOverflowError 
这也内存溢出错误的一种，即线程栈的溢出，要么是方法调用层次过多（比如存在无限递归调用），要么是线程栈太小。 

优化程序设计，减少方法调用层次；调整 -Xss 参数增加线程栈大小。


# PS

> 本文部分内容来源于网络，无法找到出处，原作者可联系邮箱 yunchow.zy@gmail.com 协商解决

