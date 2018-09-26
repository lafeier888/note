# Kafka

## kafka是一个流处理平台

​	1 发布/订阅流式记录

​	2 存储流式记录

​	3 处理流式记录

## 可以做：

1. ​	实时数据流管道

2. ​	实时流式应用程序

## 特性：

​	集群部署

​	消息按照topic分类

​	每条记录包含：key，value，timestamp

## api分类：

- ​	producer api

- ​	consumer api

- ​	streams  api

- ​	connector api	连接数据源

# Topic和Log

topic是抽象慨念，用来区分数据所属的业务类别

kafka对每一个topic维护一个分区日志

- 分区中存放的是数据序列


​		分区内每一条记录都有一个id，称为offset，可以唯一识别分区内的一条记录

​		偏移量是由消费者控制的

- 数据是存在log中的，每个分区都有一个log或者多个log（如果只有一个，越来越大...）
- 在物理结构上：分区对应文件夹，log对应文件，记录对应文件内容

kafka会保留所有的记录，被消费之后还会保存，可以配置保留时间，kafka的数据大小与性能无关

# 分布式

topic可以有多个分区
分区可以有多个副本，副本存放在不同的节点，之间存在leader/follower的关系

# 生产者

kafka只保证分区内有序，要是全局有序，用一个分区实现，一个分区就意味着在一个组里同时只有一个消费者能消费数据。

# 消费者

- kafka的消费是按分区来的，同一个分区的消息，只能一个组内的一个消费者消费

|                           | 同组           | 不同组               |
| ------------------------- | -------------- | -------------------- |
| 一条消息/一个分区内的消息 | 只有一个能消费 | 每组里只有一个能消费 |
| 多个分区的消息            | 多个消费者     | 每组多个消费者       |

 从上可以得出结论，调节消费速度的方式有：

调节消费者：

​	一个分区：没法调

​	多个分区:   增加组内消费者数量

调节分区：

​	增加分区数目

# 保证

按照生产者发送的顺序存储到日志中

消费者按照分区内记录顺序消费

一个top有N个副本，做多允许n-1台故障



# 作为消息队列使用

## 传统消息队列：

- Queue（点对点）

  一条消息只会给一个消费者，因此可以很容易实现负载均衡（扩展），可以通过增加节点减轻压力

  优点：可以扩展

  缺点：处理完就丢弃消息，无法有多个订阅者

- 发布/订阅

  一条消息会广播给所有的消费者，因此无法实现负载均衡（无法扩展），增加节点的效果没有变化

  优点：可以有多个订阅者

  缺点：无法扩展

## kafak的消息队列：

​	不同与传统的消息队列，kafka综合了传统消息队列的特点，在能够实现多个消费者的同时实现了负载均衡（扩展），实现方式是采用消费者组的概念，此时，消息是发送给所有组，每组只有一个消费者可以接收消息。



## 在并行消费消息的处理上：

​	消息按照顺序保存，但是同时存在多个消费者并行的场景下，比如多个消费者同时消费队列中的消息，

虽然消息按照存放的顺序分发非消费者，但是在发玩之后，他们的处理是异步的，不一定那个消费者先处理，因此是无法保证处理的有序的。

- 在传统消息队列中

  通过限制一个消息只有一个消费者来保证消息处理的顺序，也就是说，无法并行处理

- kafka使用了分区的概念，每个分区只会有消费者组内一个消费者消费，保证了分区内消息按顺序消费，多个分区可以并行处理。

  tops:消费者组内的消费者数不要超过分区数（一个分区，对应一个消费者，多出的会被闲置）

# 作为存储系统

数据写入，备份之后，kafka才让生产者认为写入完成

kefka可以作为：

​	高性能

​	低延迟

​	具备日志系统

​	备份和传播功能

的分布式文件系统使用。

​	

# 流处理

# 批处理



# 入门

### 启动zookeeper

bin/zookeeper-server-start.sh config/zookeeper.properties

### 启动kafka

bin/kafka-server-start.sh config/server.properties

### 创建topic

bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test

### 查看topic列表

bin/kafka-topics.sh --list --zookeeper localhost:2181

### 生产者

bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test

### 消费者

bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic test

### kafka的connector

这个东西的source就是从哪里获取数据，拿到之后存在哪个topic

sink就是把哪个topic的数据存到哪里去

```
bin/connect-standalone.sh \
config/connect-standalone.properties \
config/connect-file-source.properties \
config/connect-file-sink.properties
```

