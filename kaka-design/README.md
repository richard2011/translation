原文链接： [kafka design](http://kafka.apache.org/documentation.html#design)

译序
-----------------
kafka的设计文档，估计大家都能看到有人翻译,但是我看到的都是0.7的，但是现在已经是0.10了，其中0.8新增的副本复制，0.9的新增的配额都很重要的内容。所以我想做一次系统的整理，把该补的补一下。


概述
=======================================
我们设想Kafka会成为一个处理所有实时数据的统一平台，这个平台正是大公司所需要的。为这做到这点，我们必需要考虑很多场景。

它必须要是高吞吐以支持海量事件流，例如实时日志聚合。

它可能要平滑地处理来自离线系统周期性加载而产生的大量积压的数据。

它同时也要能够支持传统消息系统场景的低廷时消息传递。

我们想支持分区，分布式，实时处理新的消息，分发消息，这些目的使我们设计出我们的分区和消费者模型。

最后的场景是在流由其他数据系统所消费，我们很系统必须要保证当机器宕机时能够做到故障转移。

为了支持这些场景，我们需要设计一系列的独特元素，比起传统消费系统这系统更像一个数据库日志系统。我们将会介绍一下这些元素的设计。

目录
-----------------

- [译序](#译序)
- [概述](#概述)
- [第一部分：存储](part1-persistence.md)
    1. [不要害怕文件系统!](part1-persistence.md#不要害怕文件系统!])
    2. [常量时长足矣](part1-persistence.md#常量时长足矣])
- [第二部分：效率](part2-efficiency.md)
    1. [端到端压缩](part2-efficiency.md#端到端压缩])
- [第三部分：生产者](part3-producer.md)
    1. [负载均衡](part3-producer.md#负载均衡])
    2. [异步发送](part3-producer.md#异步发送])
- [第四部分：消费者](part4-consumer.md)
    1. [Push vs. pull](part4-consumer.md#Push vs. pull])
    2. [消费者Position](part4-consumer.md#消费者Position])
    3. [离线数据加载](part4-consumer.md#离线数据加载])
- [第五部分：消息传递语义](part5-message-delivery-semantics.md)
- [第六部分：副本](part6-replication.md)
    1. [选举,ISRs,和状态机](part6-replication.md#选举,ISRs,和状态机)
    2. [Leader选举:如果所有都宕机](part6-replication.md)
    3. [可用性和待久性保证](part6-replication.md)
    4. [副本管理](part6-replication.md)
- [第七部分：日志压缩](part7-log-compaction.md)
    1. [日志压缩基础](part7-log-compaction.md)
    2. [日志压缩提供什么?](part7-log-compaction.md)
    3. [日志压缩细节](part7-log-compaction.md)
    4. [配置日志清理器](part7-log-compaction.md)
    5. [局限](part7-log-compaction.md)
- [第八部分：限流](part8-quotas.md)
    1. [为什么需要限流?](part8-quotas.md)
    2. [实施](part8-quotas.md)
    3. [配额配置](part8-quotas.md)
