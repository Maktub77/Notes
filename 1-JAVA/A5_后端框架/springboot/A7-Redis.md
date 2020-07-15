## Redis

* Redis是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库。 

### 引入依赖

* Spring Boot提供的数据访问框架Spring Data Redis基于Jedis。可以通过引入`spring-boot-starter-redis`来配置依赖关系。

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-redis</artifactId>
  </dependency>
  ```

#### 参数配置

* `application.properties`中加入Redis服务端的相关配置 

  ```properties
  # REDIS (RedisProperties)
  # Redis数据库索引（默认为0）
  spring.redis.database=0
  # Redis服务器地址
  spring.redis.host=localhost
  # Redis服务器连接端口
  spring.redis.port=6379
  # Redis服务器连接密码（默认为空）
  spring.redis.password=
  # 连接池最大连接数（使用负值表示没有限制）
  spring.redis.pool.max-active=8
  # 连接池最大阻塞等待时间（使用负值表示没有限制）
  spring.redis.pool.max-wait=-1
  # 连接池中的最大空闲连接
  spring.redis.pool.max-idle=8
  # 连接池中的最小空闲连接
  spring.redis.pool.min-idle=0
  # 连接超时时间（毫秒）
  spring.redis.timeout=0
  ```

  > **其中spring.redis.database的配置通常使用0即可，Redis在配置的时候可以设置数据库数量，默认为16，可以理解为数据库的schema** 

