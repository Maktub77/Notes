##### Zookeeper

* 用于分布式中一致性处理的框架

> Netty：一个NIO框架，是对socket网络编程的优秀包装

##### Dubbo

- 一个远程调用的分布式框架

- dubbo架构

  ![1589441905771](C:\Users\Maktub_J\Documents\Notes\0-picture\dubbo架构图.png)

  1. Container(服务容器)：负责启动，加载，运行Provider（生产者）
  2. Provider(生产者)：在启动时，向注册中心注册自己提供的服务
  3. Consumer(消费者)：在启动时，向注册中心订阅自己所属的服务
  4. Registry(注册中心)：返回生产者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者
  5. 消费者从提供地址列表中，基于软负载均衡算法，选一台生产者进行调用，如果调用失败，再选另一台调用
  6. 生产者和消费者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到Monitor(监控中心)

---

###### SpringCloud全家桶