---
title: python连接kafka
date: 2015-07-14 09:56:03
categories: [Python]
tags: 
  - kafka
  - python
---
![](/media/14368436615808.jpg)

![](/media/14368437231018.jpg)

最近项目中总是跟java配合，我一个写python的程序员，面对有复杂数据结构的java代码转换成python代码，确实是一大难题，有时候或多或少会留有一点坑，看来有空还得看看java基础。这不今天又开始让我们连接kafka啦。公司的kafka跟zookeeper做了群集，连接比较麻烦，具体如何使用，java那面做的封装我也看不到，所以只能通过简单的沟通。

#开始
开始肯定去找python连接kafka的标准库，[kafka-python](https://github.com/mumrah/kafka-python)和[pykafka](https://github.com/Parsely/pykafka) 前者使用的人多是比较成熟的库，后者是Samsa的升级版本，在网上到文章[在python连接并使用kafka](http://blog.plotcup.com/a/120) 使用samsa连接zookeeper然后使用kafka Cluster很能满足我的需求，在pykafka的例子中也看到了zk的支持，而kafka-python并没有zk的支持，所以选择了pykafka做为连接库

#概念问题
kafaka和zookeeper的群集，使用samsa的时候生产者和消费者都连接了zookeeper，但是我跟峰云（大数据大牛，运维屌丝逆转）沟通，他们使用的时候是生产者直接连接kafaka服务器列表，消费者才用zookeeper。这也解决了我看pykafka文档，只有消费者才连接zookeeper的困惑，所以问题解决，直接按照文档搞起。

# 生产者

```
>>> from pykafka import KafkaClient
>>> client = KafkaClient(hosts="192.168.1.1:9092, 192.168.1.2:9092") # 可接受多个Client这是重点
>>> client.topics  # 查看所有topic
>>> topic = client.topics['my.test'] # 选择一个topic
>>> producer = topic.get_producer()
>>> producer.produce(['test message ' + str(i ** 2) for i in range(4)]) # 加了个str官方的例子py2.7跑不过
```

# 消费者

```
>>> balanced_consumer = topic.get_balanced_consumer(
    consumer_group='testgroup',
    auto_commit_enable=True,  # 设置为Flase的时候不需要添加 consumer_group
    zookeeper_connect='myZkClusterNode1.com:2181,myZkClusterNode2.com:2181/myZkChroot' # 这里就是连接多个zk
)
```

