---
layout: post
title: Notes on Distributed System
categories:
- blog
---


## Terms
* [FLP Impossibility](https://groups.csail.mit.edu/tds/papers/Lynch/jacm85.pdf), Impossibility of Distributed Consensus with One Faulty Process
* [CAP Theorem](http://en.wikipedia.org/wiki/CAP_theorem)
* [BASE: An Acid Alternative](http://queue.acm.org/detail.cfm?id=1394128)
* [Fallacies of Distributed Computing](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing)
* [Distributed systems theory for the distributed engineer](http://the-paper-trail.org/blog/distributed-systems-theory-for-the-distributed-systems-engineer/)   
Chinese version: [面向分布式系统工程师的分布式系统理论](http://blog.xiayf.cn/2014/08/10/Distributed-systems-theory-for-the-distributed-systems-engineer/)

## Books
* [Distributed Systems for fun and profit](http://book.mixu.net/distsys/single-page.html)
* [Distributed Systems Concepts and Design(Fifth Edition) - George Coulouris](https://azmuri.files.wordpress.com/2013/09/george-coulouris-distributed-systems-concepts-and-design-5th-edition.pdf)
* [Distributed Systems Principles and Paradigms - Andrew Tanenbaum](http://sist.sysu.edu.cn/~wuweig/DCC/DistributedSystemsPrinciplesandParadigms\(2nd%20edition\)-2007-Tanenbaum.pdf)
* [走向分布式](http://dcaoyuan.github.io/papers/pdfs/Scalability.pdf)

## Courses
* [Cloud Computing Concepts](https://class.coursera.org/cloudcomputing-001), University of Illinois
* [Distributed Systems - CMU fall2016](http://www.cs.cmu.edu/~srini/15-440/syllabus.html) in Go Programming Language
* [Principles of Distributed Computing - ETHZ](http://dcg.ethz.ch/lectures/podc_allstars/)
* [Distributed Systems for Information Systems Management - CMU](http://www.andrew.cmu.edu/course/95-702/)
* [Distributed Systems - MIT](https://pdos.csail.mit.edu/6.824/schedule.html)
* [Introduction to Distributed System Design - Google Code University](http://www.hpcs.cs.tsukuba.ac.jp/%7Etatebe/lecture/h23/dsys/dsd-tutorial.html)

## Papers

### Storage
* [Dynamo: Amazon's Highly Available Key Value Store](http://bnrg.eecs.berkeley.edu/%7Erandy/Courses/CS294.F07/Dynamo.pdf)
* [Bigtable: A Distributed Storage System for Structured Data](http://static.googleusercontent.com/media/research.google.com/en//archive/bigtable-osdi06.pdf)
* [The Google File System](http://static.googleusercontent.com/external_content/untrusted_dlcp/research.google.com/en/us/archive/gfs-sosp2003.pdf)
* [Cassandra: A Decentralized Structured Storage System](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.161.6751&rep=rep1&type=pdf)
* [Availability in Globally Distributed Storage Systems](http://static.googleusercontent.com/media/research.google.com/en/us/pubs/archive/36737.pdf)
* [Sheepdog: Distributed Storage System for KVM](https://github.com/ty4z2008/Qix/blob/master/ds.md)
* [PacificA: Replication in Log-Based Distributed Storage Systems](http://research.microsoft.com:8082/pubs/66814/tr-2008-25.pdf)
* [Finding a needle in Haystack: Facebook’s photo storage](https://www.usenix.org/legacy/event/osdi10/tech/full_papers/Beaver.pdf)
* [Windows Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency](http://www-bcf.usc.edu/%7Eminlanyu/teach/csci599-fall12/papers/11-calder.pdf)

### Databases
* [Spanner: Google’s Globally-Distributed Database](http://static.googleusercontent.com/media/research.google.com/zh-CN//archive/spanner-osdi2012.pdf)
* [Tera - 高性能、可伸缩的结构化数据库](https://github.com/ty4z2008/Qix/blob/master/ds.md)
* [Salt: Combining ACID and BASE in a Distributed Database](https://www.cs.utexas.edu/~lorenzo/papers/Chao14Salt.pdf)

### Resource Management
* [Apache Hadoop YARN: Yet Another Resource Negotiator](https://www.sics.se/~amir/files/download/dic/2013%20-%20Apache%20Hadoop%20YARN:%20Yet%20Another%20Resource%20Negotiator%20\(SoCC\).pdf)
* [Mesos: A Platform for Fine-Grained Resource Sharing in the Data Centeri](https://www.cs.berkeley.edu/~alig/papers/mesos.pdf)
* [Borg](https://pdos.csail.mit.edu/6.824/papers/borg.pdf), [Kubernetes](https://kubernetes.io/) an open-source implementation of Borg by [Google](http://queue.acm.org/detail.cfm?id=2898444)
### Data Processing
* [MapReduce: Simplified Data Processing on Large Clusters](https://static.googleusercontent.com/media/research.google.com/zh-CN//archive/mapreduce-osdi04.pdf)
* [Pregel: A System for Large-Scale Graph Processing](http://www.dcs.bbk.ac.uk/~dell/teaching/cc/paper/sigmod10/p135-malewicz.pdf)

### Query Processing
* [Design and Implementation of a Query Processor for a Trusted Distributed Data Base Management System](http://www.utdallas.edu/%7Ebxt043000/Publications/Journal-Papers/DAS/J16_Design_and_Implementation_of_a_Distributed_Query_Processor.pdf)

### Messaging Systems
* [The Log: What every software engineer should know about real-time data's unifying abstraction](https://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying)   
Chinese version [日志：每个软件工程师都应该知道的有关实时数据的统一概念](http://www.oschina.net/translate/log-what-every-software-engineer-should-know-about-real-time-datas-unifying?lang=chs&page=1#)
* [Kafka: a Distributed Messaging System for Log Processin](http://notes.stephenholiday.com/Kafka.pdf)

### Distributed Consensus Algorithms
* [The Part Time Parliament](http://research.microsoft.com/en-us/um/people/lamport/pubs/lamport-paxos.pdf), Lamport's original Paxos paper, but it's a bit difficult to understand.
* [Paxos Made Simple](http://research.microsoft.com/en-us/um/people/lamport/pubs/paxos-simple.pdf), a more terse readable Paxos paper by Lamport himself.
* [The Chubby Lock Service for loosely coupled distributed systems](http://static.googleusercontent.com/media/research.google.com/en//archive/chubby-osdi06.pdf)
* [Paxos made live - An engineering perspective](http://research.google.com/archive/paxos_made_live.html)
* [Raft Consensus Algorithm](https://raftconsensus.github.io/)
* [Paxos vs Raft](https://ramcloud.stanford.edu/%7Eongaro/userstudy/)

### Service Discovery
* [ZooKeeper](http://zookeeper.apache.org/)
* [Eureka!](https://github.com/Netflix/eureka)
* [Consul](https://www.consul.io/)
* [etcd](https://github.com/coreos/etcd)

### Consistency and Ordering in Distributed Systems
* [Time, Clocks, and the Ordering of Events in a Distributed System](http://research.microsoft.com/en-us/um/people/lamport/pubs/time-clocks.pdf)
* [The Byzantine Generals Problem](https://www.microsoft.com/en-us/research/publication/byzantine-generals-problem/)

### Distributed Hash Tables
* [Consistent hashing and random trees](https://www.akamai.com/es/es/multimedia/documents/technical-publication/consistent-hashing-and-random-trees-distributed-caching-protocols-for-relieving-hot-spots-on-the-world-wide-web-technical-publication.pdf)
* [Web caching with consistent hashing](http://www.cs.columbia.edu/~asherman/papers/cachePaper.pdf)
* [Chord](https://github.com/sit/dht/wiki)
* [Pastry](http://rowstron.azurewebsites.net/PAST/pastry.pdf)
* [Tapestry](https://pdos.csail.mit.edu/~strib/docs/tapestry/tapestry_jsac03.pdf)

### Replication in Distributed Databases
* [Notes on Distributed Databases](http://domino.research.ibm.com/library/cyberdig.nsf/papers/A776EC17FC2FCE73852579F100578964/$File/RJ2571.pdf)
* [ Epidemic algorithms for replicated database maintenance](https://pdfs.semanticscholar.org/49ed/15db181c74c7067ec01800fb5392411c868c.pdf)
* [Principles of Computer System Design, Chapter 10: Consistency](https://ocw.mit.edu/resources/res-6-004-principles-of-computer-system-design-an-introduction-spring-2009/online-textbook/part_ii_open_5_0.pdf)

## Blogs and other reading links
* [What are some good resources for learning about distributed computing? Why?](https://www.quora.com/What-are-some-good-resources-for-learning-about-distributed-computing-Why)


## reference
* [awesome-distributed-systems](https://github.com/kevinxhuang/awesome-distributed-systems)
* [service-discovery](https://technologyconversations.com/2015/09/08/service-discovery-zookeeper-vs-etcd-vs-consul/)
* [flp](http://blog.csdn.net/chen77716/article/details/27963079)
* [CAP理论](http://blog.csdn.net/chen77716/article/details/30635543)
* [THE BASE METHODOLOGY VERSUS THE ACID MODEL](http://www.cs.cornell.edu/courses/cs5412/2012sp/slides/VIII%20-%20BASE%20versus%20ACID.pdf)
