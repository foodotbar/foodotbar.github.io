---
layout: post
title: Ceph RBD 块设备的快照机制
categories:
- blog
---

使用Ceph为虚拟机提供块设备（RBD），则对快照（Snapshot）必不陌生。但是Snapshot和Clone的区别，以及Snapshot的原理，Ceph实现Snapshot机制所需要的数据结构，以及Snapshot读写过程是怎么实现的，则未必每个使用过Snapshot的人都能讲清楚。本文将解答上面的问题。
