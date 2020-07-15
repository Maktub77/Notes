## Spring 配置

### springBoot yml配置

1. 在 spring boot 中，有两种配置文件，一种是application.properties,另一种是application.yml,两种都可以配置spring boot 项目中的一些变量的定义，参数的设置等。下面来说说两者的区别。

   * application.properties 配置文件在写的时候要写完整

     ```properties
     spring.profiles.active=dev
     spring.datasource.data-username=root
     spring.datasource.data-password=root
     ```

   * 在yml 文件中配置的话，写法如下：

     ```yml
     spring:
       profiles:
         active: prod
       datasource:
         driver-class-name: com.mysql.jdbc.Driver
         url: jdbc:mysql://127.0.0.1:3306/test
         username: root
         password: root
     ```

   yml 文件在写的时候层次感强，而且少写了代码

2. 在项目中配置多套环境的配置方法。

   * 因为现在一个项目有好多环境，开发环境，测试环境，准生产环境，生产环境，每个环境的参数不同，所以我们就可以把每个环境的参数配置到yml文件中，这样在想用哪个环境的时候只需要在主配置文件中将用的配置文件写上就行，如下：

     ```yml
     spring:
       profiles:
         active: prod 
     ```

     这行配置在application.yml 文件中，意思是当前起作用的配置文件是application_prod.yml,其他的配置文件命名为 application_dev.yml,application_bat.yml等

3. 项目启动的时候也可以设置 Java -jar xxxxxx.jar spring.profiles.actiove=prod 也可以这样启动设置配置文件，但是这只是用于开发和测试。

4. 配置文件数据的读取：

   * 比如我在文件中配置了一个

     ```yml
     massage:
       data:
         name: qibaoyi
     ```

     我在类中想要获取他 需要这样去写：

     ```java
     @Value("${message.data.name}")
     private String name;
     ```

     后面你取到变量name 的值就是配置文件中配置的值。

5. 大家需要注意一点，配置文件中参数的写法：name: qibaoyi中间是有一个空格的，在IDEA 编译器中它会提醒你的。

### 配置文件

#### 自动配置

* Spring Boot提供了默认的配置，如默认的Bean，去运行Spring应用。它是非侵入式的，只提供一个默认实现。
* Spring Boot不单单从application.properties获取配置，所以我们可以在程序中多种设置配置属性。按照以下列表的优先级排列：
  1. 命令行参数
  2. java：comp/env里的JNDI属性
  3. JVM系统属性
  4. 操作系统环境变量
  5. `RandomValueProperty.properties(或yml)`属性类生成的random.*属性
  6. 应用以外的`application.properties(或yml)`文件
     * 在多 moudle 的项目中，比如常见的项目分 api 、service、dao 等 moudles，往往会加一个 deploy moudle 去打包该业务各个子 moudle，应用以外的配置优先。 
  7. 打包在应用内的`application.properties(或yml)文件
  8. 在应用`@Configuration配置类中，用@PropertySource注解声明的默认属性`
* 令行参数优先级最高。这个可以根据这个优先级，可以在测试或生产环境中快速地修改配置参数值，而不需要重新打包和部署应用。 

#### 自定义属性

* 项目结构

  ```xml
  ├── pom.xml
  └── src
      ├── main
      │   ├── java
      │   │   └── org
      │   │       └── spring
      │   │           └── springboot
      │   │               ├── Application.java
      │   │               └── property
      │   │                   ├── HomeProperties.java
      │   │                   └── UserProperties.java
      │   └── resources
      │       ├── application-dev.properties
      │       ├── application-prod.properties
      │       └── application.properties
      └── test
          ├── java
          │   └── org
          │       └── spring
          │           └── springboot
          │               └── property
          │                   ├── HomeProperties1.java
          │                   └── PropertiesTest.java
          └── resouorces
              └── application.yml
  ```

* 在 application.properties 中对应 HomeProperties 对象字段编写属性的 KV 值： 

  这里也可以通过占位符，进行属性之间的引用。 

  ```properties
  ## 家乡属性 Dev
  home.province=ZheJiang
  home.city=WenLing
  home.desc=dev: I'm living in ${home.province} ${home.city}.
  ```

* 编写对应的 HomeProperties Java 对象： 

  ```java
  @Component
  @ConfigurationProperties(prefix = "home")
  public class HomeProperties {
      private String province;
      private String city;
      private String desc;
      ... ...
  }
  ```

  ​	通过 @ConfigurationProperties(prefix = "home”) 注解，将配置文件中以 home 前缀的属性值自动绑定到对应的字段中。同是用 @Component 作为 Bean 注入到 Spring 容器中。 

#### random.* 属性

* Spring Boot通过`RandomValuePropertySource`提供了很多关于随机数的工具类。概括可以生成随机字符串、随机int、随机long、某随机数。

  ```yml
  ## 随机属性
  user:
    id: ${random.long}
    age: ${random.int[1,200]}
    desc: 泥瓦匠叫做${random.value}
    uuid: ${random.uuid}
  ```

#### 多环境配置

- 对于多环境的配置（数据库配置、Redis 配置、注册中心和日志配置等 

  ），各种项目构建工具或是框架的基本思路是一致的，通过配置多份不同环境的配置文件，再通过打包命令指定需要打包的内容之后进行区分打包

- 在Spring Boot中多环境配置文件名需要满足application-{profile}.properties的格式，其中{profile}对应你的环境标识，比如：

  - `application-dev.properties`：开发环境
  - `application-test.properties`：测试环境
  - `application-prod.properties`：生产环境

- 至于哪个具体的配置文件会被加载，需要在`application.properties`文件中通过`spring.profiles.active`属性来设置，其值对应`{profile}`值。 

  - 如：`spring.profiles.active=test`就会加载`application-test.properties`配置文件内容