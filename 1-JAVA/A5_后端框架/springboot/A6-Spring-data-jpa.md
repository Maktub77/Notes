* 编写一个继承自`JpaRepository`的接口就能完成数据访问 
* Spring-data-jpa依赖于Hibernate 

### 工程配置

* pom.xml

  ```xml
  <dependency
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
  </dependency>
  ```

* 在application.xml中配置：数据库连接信息（如使用嵌入式数据库则不需要）、自动创建表结构的设置，例如使用mysql的情况如下： 

  ```java
  spring.datasource.url=jdbc:mysql://localhost:3306/test
  spring.datasource.username=root
  spring.datasource.password=root
  spring.datasource.driver-class-name=com.mysql.jdbc.Driver
  
  //spring.jpa.properties.hibernate.hbm2ddl.auto是hibernate的配置属性，其主要作用是：自动创建、更新、验证数据库表结构
  spring.jpa.properties.hibernate.hbm2ddl.auto=create-drop
  ```

  hibernate的配置属性 :

  - `create`：每次加载hibernate时都会删除上一次的生成的表，然后根据你的model类再重新来生成新表，哪怕两次没有任何改变也要这样执行，这就是导致数据库表数据丢失的一个重要原因。
  - `create-drop`：每次加载hibernate时根据model类生成表，但是sessionFactory一关闭,表就自动删除。
  - `update`：最常用的属性，第一次加载hibernate时根据model类会自动建立起表的结构（前提是先建立好数据库），以后加载hibernate时根据model类自动更新表结构，即使表结构改变了但表中的行仍然存在不会删除以前的行。要注意的是当部署到服务器后，表结构是不会被马上建立起来的，是要等应用第一次运行起来后才会。
  - `validate`：每次加载hibernate时，验证创建数据库表结构，只会和数据库中的表进行比较，不会创建新表，但是会插入新值。

### 创建实体

* 创建一个User实体，包含id（主键）、name（姓名）、age（年龄）属性，通过ORM框架其会被映射到数据库表中，**由于配置了`hibernate.hbm2ddl.auto`，在应用启动的时候框架会自动去数据库中创建对应的表**。 

  ```java
  @Entity
  public class User {
      @Id
      @GeneratedValue
      private Long id;
  
      @Column(nullable = false)
      private String name;
  
      @Column(nullable = false)
      private Integer age;
      // 省略构造函数
      // 省略getter和setter
  }
  ```

### 创建数据访问接口

* 在Spring-data-jpa中，只需要编写类似下面这样的接口就可实现数据访问。

  ```java
  public interface UserRepository extends JpaRepository<User, Long> {
      User findByName(String name);
      User findByNameAndAge(String name, Integer age);
      @Query("from User u where u.name=:name")
      User findUser(@Param("name") String name);
  
  }
  ```

* 接口继承自`JpaRepository`，通过查看`JpaRepository`接口的[API文档](http://docs.spring.io/spring-data/data-jpa/docs/current/api/)，可以看到该接口本身已经实现了创建（save）、更新（save）、删除（delete）、查询（findAll、findOne）等基本操作的函数，因此对于这些基础操作的数据访问就不需要开发者再自己定义。 

* Spring-data-jpa的一大特性：**通过解析方法名创建查询**。 

* 

