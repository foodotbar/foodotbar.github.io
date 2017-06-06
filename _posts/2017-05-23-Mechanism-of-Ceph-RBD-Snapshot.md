---
layout: post
title: Ceph RBD 块设备的快照机制
categories:
- blog
---

使用Ceph的块存储功能，为虚拟机提供块设备（卷）时，我们时不时需要对块设备（卷）打快照（Snapshot）。这里将简单介绍一下Ceph中快照的基本概念，然后介绍Ceph实现快照机制所涉及的数据结构，以及Ceph快照的的工作原理及读写流程的实现。

### 1. 快照的概念
存储网络行业协会SNIA（StorageNetworking Industry Association）对快照（Snapshot）的定义是：关于指定数据集合的一个完全可用拷贝，该拷贝包括相应数据在某个时间点（拷贝开始的时间点）的映像。快照可以是其所表示的数据的一个副本，也可以是数据的一个复制品。

其实Ceph提供两种级别的快照：一种是针对Pool级别的快照，可以给整个Pool的数据做某一时刻的只读镜像；另一种是针对块设备（RBD）进行的快照。这两种快照是互斥的，不能同时共存。这里暂时只讨论针对块设备的快照。

无论Pool级别还是块设备级别的快照，Ceph实现的基本原理是相同的，基于对象的COW（copy-on-write）机制。Ceph可以实现秒级的快照操作。

![创建一个快照](http://docs.ceph.com/docs/master/_images/ditaa-75fdb48a3db2bad6ef749fdae1282f4ae2dd1f7c.png)

除了快照，Ceph还提供对块设备的克隆（Clone）操作，克隆和快照的区别在于克隆是针对某一时刻全部数据的可写镜像。克隆的实现依赖于快照机制，克隆是在块设备的一个快照的基础上实现了可写功能。快照和克隆的示意图如下。

![](http://docs.ceph.com/docs/master/_images/ditaa-b4c8b30123e5581e44b87f1836b96a869c4898b6.png)

一个镜像的克隆生成过程如下所示：首先对镜像创建快照，然后protect这个快照，然后针对这个protected的快照进行克隆操作。

![](http://docs.ceph.com/docs/hammer/_images/ditaa-bbd86247e30fd4ca83550176ab1bf5a39ab46c6d.png)

克隆的镜像有一个指向parent快照的引用，包括pool ID（这意味着可以在不同的pool之间进行克隆操作），image ID和snapshot ID。

快照和克隆的几个典型应用场景：

1. 创建镜像模板
2. 创建扩展镜像模板
3. 创建镜像模板Pool

虽然可以创建无限层级的快照和克隆，但是当层级过多时，对克隆image的读写时需要不断递归到parent image上读取对应的快照对象，就会影响克隆镜像的性能。为了解决这个问题，Ceph提供了flatten操作，一次性将parent的数据拷贝给克隆image，这样就不需要向parent image查找对象数据了，从而提高性能。

这里需要注意的一点是，对象的clone操作指的是快照对应的克隆操作，是RADOS在OSD服务端实现的对象拷贝。RBD的Clone操作是RBD的客户端实现的RBD层面的克隆。是两个不同的概念。

虽然可以在不同的Pool直接创建克隆，但是一个RBD image的数据对象和快照对象都在同一个Pool中，每个image的对象和其快照的对象都存储在相同的OSD的相同PG中。快照的对象拷贝都是在OSD本地进行的。

### 2. Ceph实现快照的核心数据结构
实现快照的核心数据结构如下：

* head对象：也就是对象的原始对象，该对象可进行写操作；
* snap对象：对某个对象做快照之后，通过cow机制copy出来的快照对象，只读；
* snap_seq / seq：快照序列号，每次做rbd snap create，monitor分配给的一个全局的序列号；
* snapdir对象：当head对象被删除之后，系统根据snap和clone对象自动创建一个snapdir对象，来保存SnapSet信息。head对象和snapdir对象互斥，同时仅有一个存在，均可保存快照相关的信息。用于文件系统的快照实现。

#### 2.1 SnapContext

在文件src/common/snap_types.h中定义了snap相关的数据结构

```
struct SnapContext {
  snapid_t seq;            // 'time' stamp 即最新的快照序列号
  vector<snapid_t> snaps;  // existent snaps, in descending order 
}
```
SnapContext数据结构用来在客户端（RBD端）保存snap相关的信息，这个结构持久化存储在RBD的元数据中。

* 其中seq为最新的快照序列号
* snaps降序保存了该RBD的所有快照序列号

librados Client中关于快照的信息定义在src/librados/IoCtxImpl.h中

```
struct librados::IoCtxImpl {
  std::atomic<uint64_t> ref_cnt = { 0 };
  RadosClient *client;
  int64_t poolid;
  snapid_t snap_seq; // 快照的id（snap id）
  ::SnapContext snapc; // snap的信息
  uint64_t assert_ver;
  version_t last_objver;
  uint32_t notify_timeout;
  object_locator_t oloc;

  Mutex aio_write_list_lock;
  ceph_tid_t aio_write_seq;
  Cond aio_write_cond;
  xlist<AioCompletionImpl*> aio_write_list;
  map<ceph_tid_t, std::list<AioCompletionImpl*> > aio_write_waiters;

  Objecter *objecter;
}
```

在IoCtxImpl中定义的snap_seq一般叫做快照的id（snap id）。当打开一个image时，如果打开的是一个卷的快照，则snap_seq为该快照的id，否则snap_seq的值为CEPH_NOSNAP，则表明操作的是卷本身。

#### 2.2 SnapSet

在 src/osd/osd_types.h 中定义了SnapSet数据结构，用来保存Server端（OSD端）与快照相关的信息

```
/*
 * attached to object head.  describes most recent snap context, and
 * set of existing clones.
 */
struct SnapSet {
  snapid_t seq;               // 最新的快照序列号
  bool head_exists;           // head对象是否存在
  vector<snapid_t> snaps;    // descending 所有的快照序列号，降序排列
  vector<snapid_t> clones;   // ascending 所有的clone对象序列号，升序排列
  map<snapid_t, interval_set<uint64_t> > clone_overlap;  
  // overlap w/ next newest 和上次clone对象之间overlap的部分
  map<snapid_t, uint64_t> clone_size; //clone对象的size
  map<snapid_t, vector<snapid_t>> clone_snaps; // descending
}
```

* seq：表示最新的快照的序列号
* head_exists：表示head对象是否存在
* snaps：保存所有快照的序列号，降序排列
* clones：表示对应快照（序列号）操作后需要拷贝的对象的记录 **{由于不是每次快照后，都需要进行对象的拷贝。只有当快照后有写操作（严格来说是第一次写），才会触发对相应对象的clone，也就是COW，复制出一份新的对象，然后再对对象进行写操作，该对象是clone出来的，其对象的快照序列号记录在clones队列中，称为clone对象}**
* clone_overlap：为每个做过clone动作的快照，记录在其clone数据对象后，原数据对象上未写过的数据部分，是采用offset~len的方式进行记录的
* clone_size：保存每次clone后的对象的size，因为有的对象一开始并不是默认的对象大小，比如默认对象大小是4MB，但是一个对象一开始只写了前2MB，所以在做完快照后进行新的写入前，这个对象就是2MB大小

SnapSet表示的信息持久化的保存在head对象的xattr扩展属性中：（此处待验证）

* 在head对象的xattr中保存key为user.ceph.snapset，value为SnapSet结构序列化的值
* 在snap对象的xattr中保存key为user.cephos.seq的snap_seq值

### 3. 快照的工作原理

#### 3.1 快照的创建

Ceph中对RBD创建一个快照的基本流程如下：

1. 客户端通过类似rbd snap create这样的命令向Monitors发送创建快照请求，获取到一个最新的快照序列号 snap_seq 值。
2. 然后将快照的 snap_name 和 snap_seq 保存到 RBD 的元数据中。

RBD的元数据保存有image所有的快照名字和对应的id，创建快照操作并不会触发OSD端的数据操作，所以是秒级。

#### 3.2 快照的写

image做了快照进行数据的写入，会触发copy-on-write（COW）机制。下面详解COW的工作机制。

写操作消息中携有SnapContext信息，包含了客户端元数据中保存的关于快照的信息，最新快照的序列号seq，以及该对象所有快照序列号的列表。在服务端，即OSD中，对象的Snap的有关信息存储在SnapSet中。写操作的处理流程按如下规则进行。

* 规则 1

如果写操作携带的SnapContext的seq值小于Server端Snapset的seq值，则直接返回错误。

可能由于多个客户端的情景下，其他客户端创建了快照，本客户端没有获取到最新的快照序列号。

Ceph有一套Watcher回调机制来实现快照序列号的更新。一个客户端对image做了快照，Server收到最新的快照序列号，会通知相应的连接客户端更新快照序列号。如果客户端没有及时更新，当写时，Server端会返回错误，此时客户端主动更新image快照的最新信息，重新发起写操作。

* 规则 2

如果head对象不存在，则创建该对象并写入数据，用SnapContext来更新SnapSet的信息。

* 规则 3

如果写请求携带的SnapContext的seq值等于SnapSet的seq值，则对head对象做正常写入。

* 规则 4 （重点）

如果写请求携带的snap_seq值大于Server端的SnapSet中的snap_seq值：

1. copy当前对象，即clone出一个新的快照对象，该快照对象的snap序列号为写请求所携带的最新的序列号，并把clone操作记录在clones列表中。
2. 用写请求携带的SnapContext中的seq值和snaps值来更新SnapSet中的seq和snaps。
3. 将数据写入到head对象中。

#### 3.3 快照的读

读取快照数据时，输入image和快照的name，通过访问客户端中RBD的元数据，获取快照对应的id，也就是sanp_seq值。

Server端，获取head的SnapSet数据。根据snaps和clones来计算所对应的正确的快照对象。

#### 3.4 快照的回滚

将head对象回滚到某个快照对象。操作如下：

1. 删除head对象
2. 拷贝相应的snap对象到head对象

#### 3.5 快照的删除

快照删除时，直接删除RBD元数据中保存的snap相关的信息，然后给Monitor发送删除信息；Monitor给相应OSD发送删除的快照序列号；然后OSD控制删除本地相对应的快照的对象。该快照是否被其他快照所共享。

Ceph快照的删除是延迟删除，并非立即删除。

### 4. Ceph实现快照读写的源码分析

to be continued

### reference
[1] http://docs.ceph.com/docs/hammer/rbd/rbd-snapshot/