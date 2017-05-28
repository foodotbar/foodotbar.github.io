---
layout: post
title: Ceph RBD 块设备的快照机制
categories:
- blog
---

Ceph提供块存储设备，而块存储设备的使用场景中，我们离不开对块设备做快照（Snapshot）。本文将简单介绍一下Ceph中快照的基本概念，然后介绍Ceph实现快照机制所涉及的数据结构，以及快照的的工作原理及读写流程的实现。

### 1. 快照的概念
快照是针对块设备某一时刻全部数据的只读镜像。
其实Ceph提供两种类型的快照：一种是针对Pool级别的快照，可以给整个Pool的数据做某一时刻的只读镜像；另一种是针对块设备（RBD）进行的快照。这两种快照是互斥的，不能同时共存，也就是如果对Pool整体做了快照，就不能对该Pool中的RBD块设备做快照了。

Pool和RBD快照，它们实现的基本原理都是相同的，基于对象的COW（copy-on-write）机制。Ceph可以实现秒级的快照操作。

Ceph还提供对块设备的克隆（Clone）操作，克隆和快照的区别在于克隆是针对某一时刻全部数据的可写镜像。克隆的实现依赖于快照机制，克隆是在块设备的一个快照的基础上实现了可写功能。快照和克隆的示意图如下，其生成过程如下所示：

```flow
op1=>operation: RBD Image
op2=>operation: Snapshot
op3=>operation: Clone

op1->op2->op3
```

一个RBD image的数据对象和快照对象都在同一个Pool中，每个image的对象和其快照的对象都存储在相同的OSD的相同PG中。快照的对象拷贝都是在OSD本地进行的。


