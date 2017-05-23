---
layout: post
title: Ceph RBD 块设备的快照机制
categories:
- blog
---

利用Ceph为虚拟机提供块存储（RBD），则快照（Snapshot）必不可少。这里简单介绍一下什么是Snapshot，以及Snapshot的原理，Ceph实现Snapshot机制所需要的数据结构，以及Snapshot读写过程进行分析来加深理解。
