---
layout: post
title: Notes on Stroage
categories:
- blog
---

## the Linux Storage Stack Diagram
![the Linux Storage Stack Diagram](https://www.thomas-krenn.com/de/wikiDE/images/e/e0/Linux-storage-stack-diagram_v4.10.png)

## Storage Devices related

### Hardware
* [Hard disk drive - HDD](https://en.wikipedia.org/wiki/Hard_disk_drive)
* [Solid-state drive - SSD](https://en.wikipedia.org/wiki/Solid-state_drive)

### Access and interfaces
* [SCSI - Small Computer System Interface](https://en.wikipedia.org/wiki/SCSI)
* [IDE - Integrated Drive Electronics / Parallel ATA](https://en.wikipedia.org/wiki/Parallel_ATA#IDE_and_ATA-1)
* [FC - Fibre Channel](https://en.wikipedia.org/wiki/Fibre_Channel)
* [SAS - Serial Attached SCSI](https://en.wikipedia.org/wiki/Serial_Attached_SCSI)
* [SATA - Serial ATA](https://en.wikipedia.org/wiki/Serial_ATA)
* [PCI Express](https://en.wikipedia.org/wiki/PCI_Express)
* [USB](https://en.wikipedia.org/wiki/USB)
* [ATAPI - ATA Packet Interface](https://en.wikipedia.org/wiki/ATA_Packet_Interface)
* [AHCI - Advanced Host Controller Interface](https://en.wikipedia.org/wiki/Advanced_Host_Controller_Interface)
* [NVMe - NVM Express](https://en.wikipedia.org/wiki/NVM_Express)

## Data Structure
* [B Tree](https://en.wikipedia.org/wiki/B-tree), implementation for Go [Google btree](https://github.com/google/btree), Original papers [Organization and Maintenance of Large Ordered Indices](http://www.minet.uni-jena.de/dbis/lehre/ws2005/dbs1/Bayer_hist.pdf) and [Binary B-trees for virtual memory](http://dl.acm.org/citation.cfm?id=1734731)
* [B+ Tree](https://en.wikipedia.org/wiki/B%2B_tree), implementation for Go [B+ tree](https://github.com/timtadh/fs2/tree/master/bptree), by C++11 [B+ Tree](http://www.amittai.com/prose/bplustree_cpp.html)
* [Merkle Tree](https://en.wikipedia.org/wiki/Merkle_tree), paper [Merkle Hash Tree based Techniques for Data Integrity of Outsourced Data](http://ceur-ws.org/Vol-1366/paper13.pdf)
* [Hast Table](https://en.wikipedia.org/wiki/Hash_table), text [Algorithms 4th](http://algs4.cs.princeton.edu/34hash/)
* [LSM-Tree / The Log-Structured Merge-Tree](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.44.2782&rep=rep1&type=pdf), papers [Diff-Index](http://researcher.ibm.com/researcher/files/us-wtan/DiffIndex-EDBT14-CR.pdf) and [bLSM](http://www.eecs.harvard.edu/~margo/cs165/papers/gp-lsm.pdf) and [Tree Indexing](https://www.cse.ust.hk/~yike/icde09s2.pdf) and [LSMTrees](https://github.com/wiredtiger/wiredtiger/wiki/LSMTrees), [blog](http://www.benstopford.com/2015/02/14/log-structured-merge-trees/)
* [COLA Cache-Oblivious Look ahead Arry](https://en.wikipedia.org/wiki/Cache-oblivious_algorithm), [paper](http://supertech.csail.mit.edu/papers/sbtree.pdf), [presentation](https://github.com/jdbeutel/ics621-proj/blob/master/downloads/bender-Scalperf-9-09.pdf), [code](https://github.com/giannitedesco/cola), [txt](https://pdfs.semanticscholar.org/abcc/8e337925ef5d8f8e348c5056256bce9b16bc.pdf)
* [Bloom filter](https://en.wikipedia.org/wiki/Bloom_filter), [original paper](http://www.lsi.upc.es/~diaz/p422-bloom.pdf), [tutorial](https://llimllib.github.io/bloomfilter-tutorial/), [app1](http://theory.stanford.edu/~matias/papers/sbf-sigmod-03.pdf), [app2](https://www.eecs.harvard.edu/~michaelm/postscripts/im2005b.pdf)
* [Bitcask](http://basho.com/wp-content/uploads/2015/05/bitcask-intro.pdf), [blog](http://highscalability.com/blog/2011/1/10/riaks-bitcask-a-log-structured-hash-table-for-fast-keyvalue.html/), [Go-case](https://laohanlinux.github.io/2016/04/25/%E5%AE%9E%E7%8E%B0%E4%B8%80%E4%B8%AA%E7%AE%80%E5%8D%95%E7%9A%84Bitcask%E5%BC%95%E6%93%8E%E7%9A%84%E5%AD%98%E5%82%A8%E7%B3%BB%E7%BB%9F/)
* [Skiplist](https://en.wikipedia.org/wiki/Skip_list), [paper](http://cglab.ca/~morin/teaching/5408/refs/p90b.pdf), [thesis](https://cs.uwaterloo.ca/research/tr/1993/28/root2side.pdf), [code](https://github.com/huandu/skiplist), [related Skip-Graphs](http://cs-www.cs.yale.edu/homes/shah/pubs/soda2003.pdf)
* [future](https://www.usenix.org/legacy/event/lisa10/tech/slides/limoncelli.pdf)

## DataStructure & Projects
* [BRTFS](), based on B+ Tree
* [MySQL](), based on B tree
* [BitTorrent](), based on Merkle Hashing Tree
* [Redis](), base on Hast
* LSM-Tree, [InfluxDB](https://www.influxdata.com/new-storage-engine-time-structured-merge-tree/), [Hbase]()

## filesystem study
* [ext2](https://en.wikipedia.org/wiki/Ext2), [Design and Implementation of the Second Extended Filesystem](http://e2fsprogs.sourceforge.net/ext2intro.html), [Ninals-GitHub/Learning-Ext2-Filesystem](https://github.com/Ninals-GitHub/Learning-Ext2-Filesystem)
* [ext3](https://en.wikipedia.org/wiki/Ext3), [design paper](http://e2fsprogs.sourceforge.net/journal-design.pdf)
* [ext4](https://en.wikipedia.org/wiki/Ext4), [Ext4 Wiki](https://ext4.wiki.kernel.org/index.php/Main_Page)
* [Btrfs](https://en.wikipedia.org/wiki/Btrfs), [BTRFS: The Linux B-tree Filesystem](https://pdfs.semanticscholar.org/fbd9/d1056ffbd18c2b53ee7abbe1521c7066df47.pdf)

## Books


## Courses

## Conferences

## reference
* [isc621-project](https://github.com/jdbeutel/ics621-proj)
