### [Redis热点key发现及常见问题](https://mp.weixin.qq.com/s?__biz=MzI4OTA3NDQ0Nw==&mid=2455545849&idx=1&sn=3416cc364836de7f478800333e65abe7&chksm=fb9cbb99cceb328fde156100b76e9106b6406b1bd38956dbff5e53bb475de5ab8d7d753caf51&scene=0#rd)

#### 热点key产生的原因

* 用户消费数据远大于生产的数据（热卖商品、热点新闻、热点评论、明星直播）
* 请求分片集中，超过单server的性能极限

#### 热点key问题的危害

![1548318044755](../../../../AppData/Local/Temp/1548318044755.png)

1. 流量集中，达到物理网卡上限
   * 一热点 Key 的请求在某一主机上超过该主机网卡上限时，由于流量的过度集中，会导致服务器中其它服务无法进行。 
2. 请求过多，缓存分片服务被打垮
   * 热点过于集中，热点 Key 的缓存过多，超过目前的缓存容量时，就会导致缓存分片服务被打垮现象的产生。 
3. DB击穿，引起业务雪崩
   * 当缓存服务崩溃后，此时再有请求产生，会缓存到后台DB上，由于DB本身性能比较弱，在面临大请求时很容易发生请求穿透现象，会进一步导致雪崩现象，严重影响设备性能。

#### 解决方案

* 通常的解决方案主要集中在对客户端和Server端相应的改造

1. **服务端缓存方案**

   ![img](https://mmbiz.qpic.cn/mmbiz_jpg/UtWdDgynLdZtFBgibAh90yNYa7zzu41SNw6YTm7wnjw3OpeKkibDDEUyXsBWhb2icLwSaj2U6YaRx9sibiafkG8Zwibg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

   * 首先Client会将请求发送至Server上，而Server又是一个多线程的服务，本地就具有一个基于Cache LRU策略的缓存空间

   * 当Server本身就拥堵时，Server不会将请求进一步发给DB而是直接返回，只有当Server本身通畅才会将Client请求发送至DB，并且将该数据重新写入到缓存中。

     此时就完成了缓存的访问跟重建

   * 问题：

     * 缓存失败、多线程构建缓存问题
     * 缓存丢失、缓存构建问题
     * 脏读问题

2. **使用Memcache、Redis方案**

   ![img](https://mmbiz.qpic.cn/mmbiz_jpg/UtWdDgynLdZtFBgibAh90yNYa7zzu41SNpibrJB4R4UynDfmTVwLEf9bRat4n4iaqbJqafGNRbcDfLsr9osB3kUQA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

   * 该方案通过在客户端单独部署缓存的方式来解决热点key问题
   * 使用过程中Client首先访问服务层，再对同一主机上的缓存进行访问
   * 优点
     * 就近访问快、没有带宽限制
   * 缺点：
     * 内存资源浪费
     * 脏读问题

3. **使用本地缓存方案**

   * 问题：
     * 需要提前获知热点
     * 缓存容量有限
     * 不一致性时间增长
     * 热点key遗漏

4. **读写分离方案解决热度**

   ![img](https://mmbiz.qpic.cn/mmbiz_jpg/UtWdDgynLdZtFBgibAh90yNYa7zzu41SNibicmE834W0fm0DMgvGb3HyIoqMvIr3Efextibd0Duof2XlibiaYOFD9tiaA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

   架构中各节点的作用：

   * SLB层做负载均衡

   * Proxy层做读写分离自动路由

   * Master负责写请求

   * ReadOnly节点负责读请求

   * Slave节点和Master节点做高可用

     > 高可用性：通常来描述一个系统经过专门的设计，从而减少停工时间，而保持其服务的高度可用性。

   实际过程中Client将请求传到SLB，SLB又将其分发至多个 Proxy 内，通过 Proxy 对请求的识别，将其进行分类发送。 

   例如，将同为 Write 的请求发送到 Master 模块内，而将 Read 的请求发送至 ReadOnly 模块。

   而模块中的只读节点可以进一步扩充，从而有效解决热点读的问题。

   读写分离同时具有可以灵活扩容读热点能力、可以存储大量热点Key、对客户端友好等优点。

5. **热点数据解决方案**

   ![img](https://mmbiz.qpic.cn/mmbiz_jpg/UtWdDgynLdZtFBgibAh90yNYa7zzu41SNbRMojIjNN3GqaNpUuY94SYmrpPAPKOpO5MIGpT9y5G1cAkZCClzX1A/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

   该方案通过主动发现热点并对其进行存储来解决热点 Key 的问题。

   首先 Client 也会访问 SLB，并且通过 SLB 将各种请求分发至 Proxy 中，Proxy 会按照基于路由的方式将请求转发至后端的 Redis 中。

   在热点 key 的解决上是采用在服务端增加缓存的方式进行。

   具体来说就是在 Proxy 上增加本地缓存，本地缓存采用 LRU 算法来缓存热点数据，后端 db 节点增加热点数据计算模块来返回热点数据。

   - Proxy 架构的主要有以下优点：
   - Proxy 本地缓存热点，读能力可水平扩展
   - DB 节点定时计算热点数据集合
   - DB 反馈 Proxy 热点数据
   - 对客户端完全透明，不需做任何兼容

#### 热点key处理

1. **热点数据的读取**

   ![img](https://mmbiz.qpic.cn/mmbiz_jpg/UtWdDgynLdZtFBgibAh90yNYa7zzu41SNUY7or1dYcg28s5UMDpBbXZwpsl3ibBX2hdWU0TyKnvCmlIWlWPFibGIw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

   在热点 Key 的处理上主要分为写入跟读取两种形式，在数据写入过程当 SLB 收到数据 K1 并将其通过某一个 Proxy 写入一个 Redis，完成数据的写入。

   假若经过**后端热点模块计算发现 K1 成为热点 key 后， Proxy 会将该热点进行缓存，当下次客户端再进行访问 K1 时，可以不经 Redis**。

   最后由于 proxy 是可以水平扩充的，因此可以任意增强热点数据的访问能力。

2. **热点数据的发现**

   ![img](https://mmbiz.qpic.cn/mmbiz_jpg/UtWdDgynLdZtFBgibAh90yNYa7zzu41SNGG19on5VFwk2YxeAzPf6D7SeiaCJOQADOGEniapibwlVOlpj7tzRUVerA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

   对于 db 上热点数据的发现，首先会在一个周期内对 Key 进行请求统计，在达到请求量级后会对热点 Key 进行热点定位，并将所有的热点 Key 放入一个小的 LRU 链表内，在通过 Proxy 请求进行访问时，若 Redis 发现待访点是一个热点，就会进入一个反馈阶段，同时对该数据进行标记。

   DB 计算热点时，主要运用的方法和优势有：

   1、基于统计阀值的热点统计

   2、基于统计周期的热点统计

   3、基于版本号实现的无需重置初值统计方法

   4、DB 计算同时具有对性能影响极其微小、内存占用极其微小等优点

   





