---
layout: post
title: 深入理解 JVM 垃圾回收原理（下）
categories: [技术]
description: 深入理解 JVM 垃圾回收原理（下）
keywords: JVM, GC, Java
---

## JVM 内存管理介绍
描述垃圾回收器如何实现内存管理，如何选择回收算法，如何设置回收区块的大小。同时介绍内存自动管理的概念，其中着重会讲解基于分代的内存管理机制，再详细描述下最常见的 4 种回收算法。

由于文章太长，分两上、下两篇文章来分别说明，这一篇是下篇

## 回收算法

### 串行回收算法
![serial_ygc.png](/imgs/serial_ygc.png)

