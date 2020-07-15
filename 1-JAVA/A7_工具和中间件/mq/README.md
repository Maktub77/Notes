## MOM

- (Message Oriented Middleware)中间件
- 有很多消息中间件
  - ActiveMQ	Java语言写的和Java系统结合紧密	
  - RabbitMQ        Erang(天生支持高并发) 非JAVA系统的首选    优于ActiveMQ
  - ZeroMQ           内存里用的   不支持持久化  金融场景  比较多见   
  - **RocketMQ**      阿里巴巴开源中间件  专门Java系统  Apache
  - Kafka              天生设计为分布式 很方便扩展 超高并发 10w+  推荐Java

------

1. 什么是消息队列？

   - "消息队列"是在消息的传输过程中保存消息的容器。

   消息：

   - "消息"是在两台计算机间传送的数据单位。消息可以非常简单，例如只包含文本字符串；也可以更复杂，可能包含嵌入对象

2. 为什么需要消息队列(MQ)?

   ​	主要原因是由于高并发环境下，由于来不及同步处理，请求往往会发生堵塞，比如说，大量的insert，update之类的请求同时到达MySQL，直接导致无数的行锁表锁，甚至最后请求会堆积过多，从而触发too many connections错误。通过使用消息队列，我们可以异步处理请求，从而缓解系统的压力。

------

## [ActiveMQ](https://www.cnblogs.com/cyfonly/p/6380860.html)

- ActiveMQ是一个MOM，具体来说就是一个实现了JMS规范的系统间远程通信的消息代理。

- MOM就是面向消息中间件(Message-oriented-middleware)，是用于以分布式应用或系统中的异步、松耦合、可靠、可拓展和安全通信的一类软件。

  MOM的总体思想是它作为消息发送器和消息接收器之间的消息中介，这种中介提供了一个全新水平的松耦合。

- JMS叫做Java消息服务(Java Message Service)，是Java平台上有关面向MOM的技术规范，旨在通过提供标准的产生、发送、接受和处理消息的API简化企业应用的开发，类似于JDBC和关系型数据库通信方式的抽象。

  - Provider：纯Java语言编写的JMS接口实现(比如ActiveMQ)
  - Domains：消息传递方式，包括点对点（P2P）、发布/订阅(Pub/Sub)
  - Connection factory：客户端使用连接工厂来创建于JMS provider的连接
  - Destination：消息被寻址、发送以及接受的对象

  ------

使用JMS创建应用程序的通用步骤

- 获取连接工厂
- 使用连接工厂创建连接
- 启动连接
- 从连接创建会话
- 获取Destination
- 创建Producer，或
  - 创建message
- 创建Consumer，发送或接受message
  - 创建Consumer
  - 注册消息监听器(可选)
- 发送或接受message
- 关闭资源(connection，session，producer，consumer等)

------

## RocketMQ

- RocketMQ是一款低延迟、高可靠、可伸缩、易于使用的消息中间件。

特性：

1. 支持发布/订阅（Pub/Sub）和点对点（P2P）消息模型
2. 在一个队列中可靠的先进先出（FIFO）和严格的顺序传递
3. 支持拉（pull）和推（push）两种消息模式
4. 单一队列百万消息的堆积能力
5. 支持多种消息协议，如JMS、MQTT等
6. 分布式高可用的部署架构，满足至少一次消息传递语义
7. 提供docker镜像用于隔离测试和云集群部署
8. 提供配置、指标和监控等功能丰富的Dashboard

### 专业术语

#### Producer

消息生产者，生产者的作用就是将消息发送到 MQ，生产者本身既可以产生消息，如读取文本信息等。也可以对外提供接口，由外部应用来调用接口，再由生产者将收到的消息发送到 MQ

#### Producer Group

生产者组，简单来说就是多个发送同一类消息的生产者称之为一个生产者组。在这里可以不用关心，只要知道有这么一个概念即可。

#### Consumer

消息消费者，简单来说，消费 MQ 上的消息的应用程序就是消费者，至于消息是否进行逻辑处理，还是直接存储到数据库等取决于业务需要。

#### Consumer Group

消费者组，和生产者类似，消费同一类消息的多个 consumer 实例组成一个消费者组。

#### Topic

Topic 是一种消息的逻辑分类，比如说你有订单类的消息，也有库存类的消息，那么就需要进行分类，一个是订单 Topic 存放订单相关的消息，一个是库存 Topic 存储库存相关的消息。

#### Message

Message 是消息的载体。一个 Message 必须指定 topic，相当于寄信的地址。Message 还有一个可选的 tag 设置，以便消费端可以基于 tag 进行过滤消息。也可以添加额外的键值对，例如你需要一个业务 key 来查找 broker 上的消息，方便在开发过程中诊断问题。

#### Tag

标签可以被认为是对 Topic 进一步细化。一般在相同业务模块中通过引入标签来标记不同用途的消息。

#### Broker

Broker 是 RocketMQ 系统的主要角色，其实就是前面一直说的 MQ。Broker 接收来自生产者的消息，储存以及为消费者拉取消息的请求做好准备。

#### Name Server

Name Server 为 producer 和 consumer 提供路由信息

### RocketMQ架构

### ![img](https://upload-images.jianshu.io/upload_images/6332814-7885601f065a3556.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/975/format/webp)

四个集群：

1. NameServer集群：提供轻量级的服务发现和路由。 每个 NameServer 记录完整的路由信息，提供等效的读写服务，并支持快速存储扩展。
2. Broker集群：通过提供轻量级的 Topic 和 Queue 机制来处理消息存储,同时支持推（push）和拉（pull）模式以及主从结构的容错机制。
3. Producer集群：生产者，产生消息的实例，拥有相同 Producer Group 的 Producer 组成一个集群。
4. Consumer集群：消费者，接收消息进行消费的实例，拥有相同 Consumer Group 的
   Consumer 组成一个集群。

简单说明一下图中箭头含义，从 Broker 开始，Broker Master1 和 Broker Slave1 是主从结构，它们之间会进行数据同步，即 Date Sync。同时每个 Broker 与
 NameServer 集群中的所有节
 点建立长连接，定时注册 Topic 信息到所有 NameServer 中。

Producer 与 NameServer 集群中的其中一个节点（随机选择）建立长连接，定期从 NameServer 获取 Topic 路由信息，并向提供 Topic 服务的 Broker Master 建立长连接，且定时向 Broker 发送心跳。Producer 只能将消息发送到 Broker master，但是 Consumer 则不一样，它同时和提供 Topic 服务的 Master 和 Slave
 建立长连接，既可以从 Broker Master 订阅消息，也可以从 Broker Slave 订阅消息。

### RocketMQ集群部署模式

1. 单master模式

   也就是只有一个 master 节点，称不上是集群，一旦这个 master 节点宕机，那么整个服务就不可用，适合个人学习使用

2. 多master模式

   多个 master 节点组成集群，单个 master 节点宕机或者重启对应用没有影响。
    优点：所有模式中性能最高
    缺点：单个 master 节点宕机期间，未被消费的消息在节点恢复之前不可用，消息的实时性就受到影响。
    **注意**：使用同步刷盘可以保证消息不丢失，同时 Topic 相对应的 queue 应该分布在集群中各个节点，而不是只在某各节点上，否则，该节点宕机会对订阅该 topic 的应用造成影响。

3. 多master多slave异步复制模式

   在多 master 模式的基础上，每个 master 节点都有至少一个对应的 slave。master
    节点可读可写，但是 slave 只能读不能写，类似于 mysql 的主备模式。
    优点： 在 master 宕机时，消费者可以从 slave 读取消息，消息的实时性不会受影响，性能几乎和多 master 一样。
    缺点：使用异步复制的同步方式有可能会有消息丢失的问题。

4. 多 master 多 slave 同步双写模式
   同多 master 多 slave 异步复制模式类似，区别在于 master 和 slave 之间的数据同步方式。
   优点：同步双写的同步模式能保证数据不丢失。
   缺点：发送单个消息 RT 会略长，性能相比异步复制低10%左右。
   刷盘策略：同步刷盘和异步刷盘（指的是节点自身数据是同步还是异步存储）
   同步方式：同步双写和异步复制（指的一组 master 和 slave 之间数据的同步）
   **注意**：要保证数据可靠，需采用同步刷盘和同步双写的方式，但性能会较其他方式低。