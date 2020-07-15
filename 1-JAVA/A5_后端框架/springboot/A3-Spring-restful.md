## Restful

* Springboot 实现 Restful 服务，基于 HTTP / JSON 传输，适用于前后端分离。 

### 运行springboot-restful工程

1. 数据库准备

2. springboot-restful工程结构介绍

   ```java
   org.spring.springboot.controller - Controller 层
   org.spring.springboot.dao - 数据操作层 DAO
   org.spring.springboot.domain - 实体类
   org.spring.springboot.service - 业务逻辑层
   Application - 应用启动类
   application.properties - 应用配置文件，应用启动会自动读取配置
   ```

3. 改数据库配置

   * 打开 application.properties 文件， 修改相应的数据源配置，比如数据源地址、账号、密码等。（如果不是用 MySQL，自行添加连接驱动 pom，然后修改驱动名配置。） 

4. 编译

   * 在项目根目录 springboot-learning-example，运行 maven 指令：

     mvn clean install

5. 右键运行 springboot-restful 工程 Application 应用启动类的 main 函数。

   用 postman 工具可以如下操作，

   根据 ID，获取城市信息

   GET http://127.0.0.1:8080/api/city/1

---

### springboot-restful 工程控制层实现详解

1. 什么是REST
   * REST是属于WEB自身的一种架构风格。
   * `Representational State Transfer`：表现层转化
   * 五个关键要素
     * 资源（Resource）：比如newsfeed 
     * 资源的表述（Representation）：比如用JSON，富文本等 
     * 状态变化（State Transfer）
     * 统一接口（Uniform Interface）
     * 超文本驱动（Hypertext Driven）：通过HTTP 动作实现 
   * 六个主要特性：
     * 面向资源（Resource Oriented）
     * 可寻址（Addressability）
     * 连通性（Connectedness）
     * 无状态（Statelessness）
     * 统一接口（Uniform Interface）
     * 超文本驱动（Hypertext Driven）
2. Spring对REST支持实现
   * @RequestMapping处理请求地址映射
     * method-指定请求方法类型：POST/GET/DELETE/PUT等
     * value-指定实际请求地址
     * consumes-指定处理请求的提交类型，如Content-Type头部
     * produces-指定返回的内容类型
   * @PathVariable URL映射时，用于绑定请求参数到方法参数
   * RequestBody 这里注解用于读取请求体的数据，通过HttpMessageConverter解析绑定到对象中
3. HTTP知识补充
   * GET            请求获取Request-URI所标识的资源 
   * POST          在Request-URI所标识的资源后附加新的数据 
   * HEAD         请求获取由Request-URI所标识的资源的响应消息报头 
   * PUT            请求服务器存储一个资源，并用Request-URI作为其标识
   * DELETE       请求服务器删除Request-URI所标识的资
   * TRACE        请求服务器回送收到的请求信息，主要用于测试或诊断
   * CONNECT  保留将来使用
   * OPTIONS   请求查询服务器的性能，或者查询与资源相关的选项和需求

