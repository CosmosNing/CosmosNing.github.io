---
title: jvm-notes
toc: true
mathjax: true
permalink: jvm-notes/
date: 2023-07-23 22:47:48
description: 整理JVM常见知识点
tags:
- Java
- JVM
categories:
- 笔记
---

## 参数

* 内存相关
  * -Xmn : 新生代/年轻代（new）的值
  * -Xms：堆内存初始值（start）
  * -Xmx:堆内存最大值（max）
  * -Xss：线程栈（stack）的内存大小
  * -XX:MaxDirectMemorySize：堆外内存最大值
    * 最大堆外内存大小，此参数的含义是当Direct ByteBuffer分配的堆外内存到达指定大小后就触发Full GC
  * -XX:MaxPermSize：方法区/非堆（永久代）最大值
* 编译相关
  * -XX:CompileThreshold：JIT方法计数器阈值（计数器阈值一旦溢出，就会触发即时编译）
  * -Xmixed：默认编译模式，混合模式，即开始解释，对热点代码实行检测与编译（启动速度较快）
  * -Xint：解释模式（启动很快，执行稍慢）
  * -Xcomp：编译模式（启动很慢，执行很快）

## 工具

* jstat：虚拟机统计信息监视工具（主要用于监控GC数据信息）

  * jstat（JVM Statistics Monitoring Tool）是用于监视虚拟机各种运行状态信息的命令行工具。它可以显示本地或者远程虚拟机进程中的类加载、内存、垃圾收集、即时编译等运行时数据，在没有GUI图形界面、只提供了纯文本控制台环境的服务器上，它将是运行期定位虚拟机性能问题的常用工具。

  * jstat命令格式为

    * ```bash
      jstat [ option vmid [interval[s|ms] [count]] ]
      ```

    * 参数interval和count代表查询间隔和次数，如果省略这2个参数，说明只查询一次。

    * 选项option代表用户希望查询的虚拟机信息，主要分为三类：**类加载、垃圾收集、运行期编译状况**。

* jinfo：Java配置信息工具

  * 实时**查看**和**调整**虚拟机各项参数

  * 使用 **jps命令的-v参数可以查看虚拟机启动时显式指定的参数列表**

  * 命令格式

    * ```bash
      jinfo [ option ] pid
      ```

* jmap：Java内存映像工具

  * 命令用于生成堆转储快照（一般称为heapdump或dump文件）

  * 还可以查询finalize执行队列、Java堆和方法区的详细信息，如空间使用率、当前用的是哪种收集器等。

  * ```bash
    jmap [ option ] vmid
    ```

* jhat：虚拟机堆转储快照分析工具

  * jhat（JVM Heap Analysis Tool）命令与jmap搭配使用，来分析jmap生成的堆转储快照。

* jstack：Java堆栈跟踪工具

  * 用于生成虚拟机当前时刻的线程快照（一般称为threaddump或者javacore文件）
  * 生成线程快照的目的通常是**定位线程出现长时间停顿的原因**，如**线程间死锁、死循环、请求外部资源导致的长时间挂起**等，都是导致线程长时间停顿的常见原因。线程出现停顿时通过jstack来查看各个线程的调用堆栈，就可以获知没有响应的线程到底在后台做些什么事情，或者等待着什么资源。

## 参考

* 《深入理解Java虚拟机》