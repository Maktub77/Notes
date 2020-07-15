`EHcache `

- 轻量级缓存 它是运行在单机内存里 
  - 优点：方便简易 
  - 缺点：容量非常有限 应用场合主要是单机应用  单机范围内

`Mem cache`

- 存储内容单一 就是字符串

`Redis`

- 存储类型丰富 性能非常高 可靠性很高    排行榜

___

## Redis

* Redis是基于内存，也可以是基于磁盘持久化nosql数据库，使用C语言开发。
* 数据存储结构：key-value

### Redis命令

* redis是一种高级的key:value存储系统，其中value支持五种数据类型：

  1. 字符串（strings）

  2. 字符串列表（lists）

  3. 字符串集合（sets）

  4. 有序字符串集合（sorted sets）

  5. 哈希（hashes）

* 关于key需要注意：

  1. key不要太长，尽量不要超过1024字节，这不仅仅消耗内存，而且会降低查找的效率；
  2. key也不要太短，太短的话，ke的可读性会降低；
  3. 在一个项目中，key最好使用统一的命名模式，例如user:10000:passwd

#### Strings

* Redis存储结构是key:value，value是strings数据类型。

* 命令：

* 语法：set key value

  1. set name xiaoming

     给String类型key是name添加一个值

     **setnx**可以作为分布式锁

  2. get name

     获取key是name属性的值

  3. incr age

     给数字字符类型自动加1

     把数字字符类型自动转换成integer类型，然后执行加上1

  4. decr age

     给数字字符类型自动减1

     把数字字符类型自动转换成integer类型，然后执行减去1

  5. incrby age 10

     给指定键值加上10

  6. decrby age 10

     给指定键值减去10 

#### Hash

* Hash是集合类型，适合用来存储对象。Java集合类型是用来存储对象。

* 存储数据结构分析：

  * 第一种数据结构：![img](https://images2015.cnblogs.com/blog/943689/201608/943689-20160821171329948-915353502.jpg)

    * 存取对象，使用一个key获取一个对象，必须使用反序列化。
    * 缺点：占用IO资源。

  * 第二种数据结构：![img](https://images2015.cnblogs.com/blog/943689/201608/943689-20160821171336355-1070344829.jpg)

    * 缺点：用户ID被多次使用，数据冗余，资源浪费。

  * 第三种数据结构：![img](https://images2015.cnblogs.com/blog/943689/201608/943689-20160821171342308-705657594.jpg)
    * Redis存储结构：key是用户ID，value就是hash类型数据

    命令：

    1. hset user username xiaoming

       给user中username属性设置一个值

    2. hget user username

    3. hdel user password .....

       删除user中的属性

    4. hsetnx user email 123@qq.com

       如果user中email属性值已经存在，不会覆盖

       如果不存在，则设置值

    5. hmset user password 123 age 11

       同时设置多个值

    6. hmget user username age password

#### List

* List集合数据结构：类似数组，数据是顺序存储。
* List集合链表结构：通过指针从头指针查询到尾指针查找元素。

命令：

1. lpush mylist a b c d

   给list类型数据结构设置多个值

2. lrange mylist 0 -1

   获取mylist集合中所有值

   0：值链表开始位置

   -1：链表的结束位置

3. lpop mylist

   出栈集合mylist:出栈链表头指针元素

4. lrem mylist 3 a

   删除链表mylist中前3个等于a的值

5. lset mylist 2 s

   给链表mylist集合中2角标位置设置一个值，覆盖原值

6. linsert mylist after s b

   在集合链表mylist中s元素后面插入一个b

#### Set

命令：

1. sadd myset a b c

   给set集合myset设置值： a b c

   set集合元素值不允许重复

2. smembers myset

   获取集合myset中值

3. srem myset a b

   删除集合myset中的元素

4. smove myset myset1 c

   把集合myset中的元素c移动到集合myset1中

#### Sorted set

* set集合：有序集合。

  * 给set集合中的每一元素都设置一个得分，根据得分排序。
  * set集合元素不允许重复，得分可以重复。
  * 设置得分语法ZADD key score member \[score][member]

  命令：

  1. zadd mysset 1 one 2 two 12 three 9 four  10 five

     给集合mysset设置五个元素，每一个元素都设置一个得分

  2. zcount mysset 1 10

     获取分数1到10的元素个数，默认是闭区间

  3. zcount mysset (1 10

     获取分数1到10的元素个数，左开右闭

  4. zcount mysset -inf +inf

     获取所有元素

     -inf:最低值

     +inf:最高值

  5. zrange mysset 0 -1 withscores

     获取集合mysset中的所有元素

     0：头部元素

     -1:尾部元素

     withscores：查询元素的时候，把分数查询出来

  6. zrangebyscore mysset 1 10 withscores limit 2 2

     根据分数大小来获取元素

     limit分页获取值

### 多数据库实例

* Redis支持16个数据库实例，数据库实例角标0-15，使用redis客户端登录redis服务器，默认登录0号数据库

* 登录其他数据库实例：

  * 登录语法select 数据库实例ID(角标)

    127.0.0.1:6379>select 1

    OK

    127.0.0.1:6379[1]>

    把0号数据库数据移动1号数据库

    命令：move user 1	//移动user到1号数据库

### 事务

事务命令：

* 开启事务： multi
* 提交事务：exec
* 回滚事务：discard
* 监听事务：watch(乐观锁)

用户A,用户B 同时读取商品数量itemNum=1，用户A,用户B都需要购买商品，商品数量需要减去1，使用乐观锁解决问题

#### 事务一致性

设计：商品数量添加，在添加过程中出一个错误，查看程序执行一致性。

>特点：redis事务不遵循ACID大一统理论，即使中间执行出现错误，后面不影响执行

#### Watch

* Watch监听事务：乐观锁
* 乐观锁：一旦发现被监听事务变量发生了变化，事务回滚。

### Redis持久化

#### Rdb(redis系统默认持久化策略)

**优点：**

1. 持久化文件将只包含一个文件
2. 对灾难恢复，主从复制 效率比较高
3. 持久化工作：子进程

**缺点：**

1. 数据安全性不是很好

Rdb数据持久化同步策略

* 缺省情况下，Redis会将数据集的快照dump到dump.rdb文件中。此外，我们也可以通过配置文件来修改Redis服务器dump快照的频率，在打开redis.conf文件之后，我们搜索save，可以看到下面的配置信息：
  ​    save 900 1              //在900秒(15分钟）后，如果至少有1个key发生变化，则dump内存快照
  ​    save 300 10            //在300秒(5分钟）后，如果至少有10个key发生变化，则dump内存快照
  ​    save 60 10000        //在60秒(1分钟）后，如果至少有10000个key发生变化，则dump内存快照

### Aof持久化

**优点：**

* 根据redis aof同步策略，数据有更高安全性

**缺点：**

* 性能比Rdb低

----

Redis发布(`sunscribe`)/订阅(`public`)

异步 解耦

