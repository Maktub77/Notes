## MongoDB

* 是一个基于分布式文件存储的数据库，它是一个介于关系数据库和非关系数据库之间的产品，其主要目标是在键/值存储方式（提供了高性能和高伸缩性）和传统的RDBMS系统（具有丰富的功能）之间架起一座桥梁，它集两者的优势于一身。
* MongoDB支持的数据结构非常松散，是类似json的bson格式，因此可以存储比较复杂的数据结构，也因为他的存储格式也使得它所存储的数据在Nodejs程序应用中使用非常流畅。
* MongoDB的查询语言非常强大，其语法有点类似于面向对象的查询语言，几乎可以实现类似关系数据库单表查询的绝大部分功能，而且还支持数据建立索引。
* 同MySQL等关系数据库相比，它们在针对不同的数据类型和事务要求上都存在自己独特的优势。在数据存储的选择中，坚持多样化原则，选择更好更经济的方式，而不是自上而下的统一化。
* 我们可以直接用MongoDB来存储键值对类型数据，如：验证码，Session等；
  * 由于MongoDB的横向扩展能力，也可以用来存储数据规模在未来变得非常巨大的数据，如：日志、评论等；
  * 由于MongoDB存储数据弱类型，也可以用来存储一些多变json数据：如：与外系统交互时经常变化的JSON报文
* 对于一些对数据有复杂的高事务性要求的操作，如：账户交易等就不适合使用MongoDB来存储

---

### Spring Boot

* 在Spring Boot中，对如此受欢迎的MongoDB，同样提供了自配置功能。

#### 引入依赖

*  Spring Boot中可以通过在`pom.xml`中加入`spring-boot-starter-data-mongodb`引入对mongodb的访问支持依赖 。它的实现依赖`spring-data-mongodb` 

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-mongodb</artifactId>
  </dependency>
  ```

#### 快速开始使用Spring-data-mongodb

* 若MongoDB的安装配置采用默认端口，那么在自动配置的情况下，我们不需要做任何参数配置，就能马上连接上本地的MongoDB。下面直接使用`spring-data-mongodb`来尝试对mongodb的存取操作。（记得mongod启动您的mongodb） 

#### 参数配置

* 我们可以轻而易举的对MongoDB进行访问，但是实战中，应用服务器与MongoDB通常不会部署在同一台设备之上，这样就无法使用自动化的本地配置来进行使用。

* 我们可以方便的配置来完成支持，只需要在application.properties中加入mongodb服务端的相关配置

  ```yml
  spring.data.mongodb.uri=mongodb://name:pass@localhost:27017/test
  ```

  >*在尝试此配置时，记得MOngoDB中对test库创建具备读写权限的用户（用户名为name，密码为pass），不同版本的用户创建语句不同，注意查看文档做好准备工作*

若使用mongodb 2.x，也可以通过如下参数配置，该方式不支持mongodb 3.x。 

```yml
spring.data.mongodb.host=localhost spring.data.mongodb.port=27017
```

