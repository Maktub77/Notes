## Spring

spring4.0版本中，共包含20个不同的模块，可以划分为6类不同的功能

* **Spring核心容器**：管理着spring应用中bean的创建、配置和管理；包括bean工厂和应用上下文。还包括一些企业服务，如E-mail、JNDI访问、EJB集成与调度

  * Beans
  * Core
  * Context
  * Expression
  * Context support

* **Spring的AOP模块(面向切面编程)**：Spring对面向切面编程提供了丰富的支持。是Spring中开发切面的基础

  * AOP
  * Aspects

* **数据访问与集成(Spring的JDBC和DAO模块)**：抽象了JDBC的样板是代码，使数据库代码更加简洁明了。

  除了JDBC，Spring还提供了对于ORM的支持,Spring的ORM模块建立在对DAO的支持智商，并为多个ORM框架提供了一种构建DAO的简便方式。Spring对许多流行的ORM框架进行了集成，包括Hibernate、Java Persisternce API、Java Data Object和iBatics SQL Maps

  * JDBC
  * Transaction
  * ORM(Object/Relation mapping)
  * OXM
  * Messaging
  * JMS

* **web与远程调用**：该模块为web提供了MVC框架

  另外，该模块还提供了多种构建与其他应用交互的远程调用方案。Spring远程调用功能集成了RMI(Remote Method Invocation)、Hession、Burlap、JAX_WS，同时Spring还自带了一个远程调用框架Http invoker。Spring还提供了暴露和使用REST API的良好支持。

  * Web
  * Web servlet
  * Web portlet
  * WebSocket

* **Instrumentation**：该模块提供了为JVM添加代理的功能（较少使用）

  * Instrument
  * Instrument Tomcat

* **测试**：Spring为使用JNDI、Servlet和Portlet编写单元测试提供了一系列的mock对象实现

  对于集成测试，该模块为加载Spring应用上下文中的bean集合以及与Spring上下文中的bean进行交互提供了支持

  * Test

### Spring概念

* 思想
  * IoC：对象的创建以及依赖关系可以由spring完成创建以及注入
  * DI：实现IoC思想需要DI做支持
    * 注入方式：
      * set方法注入
      * 构造方法注入
      * 字段注入
    * 注入类型：
      * 值类型注入：8大基本数据类型
      * 引用类型注入：将依赖对象注入
* ApplicationContext&BeanFactory
  * BeanFactory接口
    * Spring原始接口，针对原始接口的实现类功能较为单一
    * BeanFactory接口实现类的容器，特点是每次在获得对象时才会创建对象
  * ApplicationContext
    * 每次容器启动时就会创建容器中配置的所有对象，并提供更多的功能
    * 从类路径下加载配置文件：ClassPathXmlApplicationContext
    * 从硬盘绝对路径下加载配置文件：`FileSystemXmlApplicationContext("d:/xxx/yyy/xxx")`
  * 结论：web开发中，使用ApplicationContext，在资源匮乏的环境可以使用BeanFactory

### xml中配置Bean

* 在ApplicationContext.xml文件中使用bean节点配置bean,bean属性在IoC容器中必须是唯一的

* DI：依赖注入

  * 依赖于IoC(控制反转，将对象交给Spring管理)

* 属性（setter）注入与构造器注入

  ```xml
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:p="http://www.springframework.org/schema/p"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
                            http://www.springframework.org/schema/beans/spring-beans-4.2.xsd">
  
      <!-- 
      constructor-arg
      index 	构造器变量下标
      type 	构造器变量类型
      name	构造器变量名称
      value	构造器变量值
      ref 	构造器pojo类型变量值
      map
      set
      properties
      -->
      <bean class="com.softeem.pojo.User" id="helloSpring">
          <!-- setter注入 -->
          <!-- <property name="name" value="aaa"></property> -->
          
          <!-- 构造器注入 -->
          <!-- 使用构造器注入属性值可以指定参数的位置和参数的类型，以区分重载的构造器注入在<constructor-arg>元素里声明属性 -->
          <!-- <constructor-arg name="name " value="aaa"></constructor-arg> -->
          <constructor-arg index="0" type="java.lang.String" name="name" value="bbb"></constructor-arg>
          <constructor-arg index="1" type="java.lang.String" name="sex" value="man"></constructor-arg>
          <constructor-arg index="2" type="int" name="age" value="100"></constructor-arg>
          <constructor-arg index="3" type="com.softeem.pojo.Car" name="car" ref="car1"></constructor-arg>
      </bean>
  
      <bean class="com.softeem.pojo.Car" name="car1">
          <property name="name" value="qq"></property>
          <property name="color" value="red"></property>
      </bean>
  
      <bean class="com.softeem.pojo.Car" name="car2">
          <property name="name" value="11"></property>
          <property name="color" value="red"></property>
      </bean>
  </beans>
  ```

* 集合属性

  * 当注入的属性时集合时，Spring也提供了通过一组内置的xml标签来配置集合属性（<list>,<set>,<map>）

  * List（Set和list相同）

    ```xml
    <!-- 测试如何配置集合属性 -->
    <bean class="com.softeem.pojo.User" id="user">
        <property name="name" value="aa"></property>
        <property name="age" value="10"></property>
        <property name="sex" value="man"></property>
        <property name="cars">
            <!-- 使用list节点为list类型的属性赋值 -->
            <list>
                <ref bean="car1"/>
                <ref bean="car2"/>
            </list>
        </property>
    </bean>
    ```

  * Map

    ```xml
    <!-- 配置Map属性值 -->
    <bean class="com.softeem.pojo.User" id="user1">
        <property name="name" value="aa"></property>
        <property name="age" value="10"></property>
        <property name="sex" value="man"></property>
        <property name="cars">
            <!-- 使用map节点及map的entry子节点配置map类型的成员变量-->
            <map>
                <entry key="aa" value-ref="car1"></entry>
                <entry key="bb" value-ref="car2"></entry>
            </map>
        </property>
    </bean>
    ```

* 可以使用专门的<null/>元素标签为Bean的字符串或其他对象类型的属性注入null值。

  和Struts、Hiberante等框架一样，Spring支持联级元素(当两个bean关联时，从一个bean给另一个bean赋值)的配置。但属性需要先初始化。

  ```xml
  <bean class="com.softeem.pojo.User" id="helloSpring">
      <constructor-arg index="0" type="java.lang.String" name="name" value="bbb"></constructor-arg>
      <constructor-arg index="1" type="java.lang.String" name="sex" value="man"></constructor-arg>
      <constructor-arg index="2" type="int" name="age" value="100"></constructor-arg>
      <!-- <constructor-arg index="3" type="com.softeem.pojo.Car" name="car" ref="car1"></constructor-arg> -->
      <!-- 测试赋值null -->
      <constructor-arg><null/></constructor-arg>
      <constructor-arg ref="car"></constructor-arg>
  	<!-- 为联级属性赋值，注意属性需要先初始化后在可以为联级元素赋值，否则会有异常，和Struts不同 -->
      <property name="car.name" value="qq"></property>
      <property name="car.color" value="red"></property>
  </bean>
  ```

  > 如果字面值包含特殊字符，可以使用<![CDATE[]]>包裹起来

* 内部bean

  ```xml
  <bean class="com.softeem.pojo.Car" name="car2">
      <property name="name" value="11"></property>
      <property name="color" value="red"></property>
  
      <!-- 内部bean,不能被外部引用，只能在内部引用 -->
      <property name="car3">
          <bean class="com.softeem.pojo.Car">
              <property name="name" value="11"></property>
              <property name="color" value="red"></property>
          </bean>
      </property>
  </bean>
  ```

* 使用p命名空间

  ```xml
  xmlns:p="http://www.springframework.org/schema/p"
  
  <!-- 通过p命名空间为bean属性赋值，需要先导入以上p命名空间 -->
  <bean class="com.softeem.pojo.User" id="user2" p:name="ccc" p:age="12" p:car-ref="car1"></bean>
  ```

* Bean属性

  ```xml
  <!-- scope属性 -->
  <!-- singleton:单例对象 -->
  <bean name="user1" class="com.softeem.pojo.User" scope="singleton">
      <property name="name" value="李白"></property>
  </bean>
  
  <!-- prototype:多例原型 -->
  <bean name="user2" class="com.softeem.pojo.User" scope="prototype" >
      <property name="name" value="李白"></property>
  </bean>
  
  <!-- 生命周期属性 -->
  <!-- init-method：生命周期初始化的方法 -->
  <!-- destroy-method：生命周期销毁的方法  单例模式下才会执行-->
  <bean name="user3" class="com.softeem.pojo.User" scope="singleton" init-method="init" destroy-method="destory">
      <property name="name" value="李白"></property>
  </bean>
  
  <!-- 配置静态工厂 -->
  <bean name="userFactory" class="com.softeem.pojo.UserFactory" factory-method="createUser"></bean>
  
  <!-- 配置动态工厂 -->
  <bean name="userFactory2" class="com.softeem.pojo.UserFactory2" factory-method="getUserFactory2" ></bean>
  <bean name="createUser2" factory-bean="userFactory2" factory-method="createUser"></bean>
  ```

### Spring注解

#### 使用注解配置spring

1. 导包

2. 为主配置文件引入新的命名空间（约束）

   ```xml
   xmlns:context="http://www.springframework.org/schema/context" 
   ```

3. 在类中使用注解完成配置

   ```xml
   <!-- 开启使用注解代理配置文件 -->
   <!-- component-scan:类扫描路径 -->
   <!-- 自动扫描com.softeem.pojo包及其子包下被@component(或者其拓展)表中的类，并把他们注册为Bean -->
   <context:component-scan base-package="com.softeem.pojo" </context:component-scan>
   ```

* 将对象注册到容器

  * @Component("user")：标注一个普通的spring bean
  * @Service("user")：标注一个业务逻辑组件类(service层)
  * @Controller("user")：标注一个控件组件类(web层)
  * @Repository("user")：标注一个DAO组件类(DAO层)

* 指定对象的作用范围

  * @Scope("prototype")

    * singleton单例 
    * prototype多例

    >  整合struts2时,ActionBean必须配置为多例的.

* 值类型注入

  * @Value("tom")

    * 通过反射Field赋值,破坏了封装性

      ```java
      @Value("tom") 
      private String name;
      ```

    * 通过set方法赋值，推荐使用

      ```java
      @Value("tom") 
      public void setName(String name) {
          this.name = name;
      }
      ```

* 引用类型注入

  * @Autowired：自动装配

    > 如果匹配多个类型一致的对象，将无法选择具体注入哪一个对象

    ```java
    @Autowired
    private String name;
    ```

  * @Qualifier：告诉Spring容器自动装配那个名称的对象

    ```java
    @Autowired
    @Qualifier("car")
    private Car cars;
    ```

  * @Resource(name="car")：手动注入，指定注入那个名称的对象

    ```java
    @Resource(name="car")
    private Car cars;
    ```

* 初始化/销毁方法

  * PostConstruct：在对象被创建后调用.init-method

    ```java
    @PostConstruct
    public void init(){
        System.out.println("初始化");
    }
    ```

  * PreDestroy：在销毁之前调用.destroy-method

    ```java
    @PreDestroy
    public void destory(){
        System.out.println("销毁");
    }
    ```

#### Spring与Junit整合测试

1. 导包

2. 配置注解

   * @RunWith(SpringJUnit4ClassRunner.class)：帮我们创建容器
   * @ContextConfiguration()：指定创建容器时使用那个配置文件

3. 测试

   ```java
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration("classpath:applicationContext.xml")
   public class SpringTest {
   
       @Resource(name="user")
       private User user;
   
       @Test
       public void getUser(){
           System.out.println(user);
       }
   }
   ```


### Spring中的AOP

* aop思想介绍：横向重复，纵向抽取

  * Filter：解决乱码

  * InvocationHandler：管理事务

    ```java
    Proxy.newProxyInxtance(classLoader,Interface[] arr,InvocationHandeler handler)
    ```

  * 拦截器：参数赋值

* Aspect：切面，在实际应用中，切面通常是指封装的用于横向插入系统功能（如事物、日志等）的类。

* Joinpoint：连接点，在程序执行过程中的某个阶段点，它实际上是对象的一个操作，例如方法的调用或异常的抛出。

* Pointcut：切入点，是指切面与程序流程的交叉点，即那些需要处理的连接点。通常在程序中，切入点指的是类或者方法名，如某个通知要应用到所有以add开头的方法中，那么所有满足这一规则的方法都是切入点

* Advice：通知/增强处理，AOP框架在特定的切入点执行的增强处理，即在定义好的切入点处所要执行的程序代码。可以将其理解为切面类中的方法，它是切面的具体实现

* Target Object：目标对象，是指所有被通知的对象，也称为被增强对象。如果AOP框架采用的是动态的AOP实现，那么该对象就是一个被代理的对象。

* Proxy：代理，将通知应用到目标对象之后，被动态创建的对象

* Weaving：织入，将切面代码插入到目标对象上，从而生成代理对象的过程

---

### IoC/DI

#### IoC(控制反转)

- 控制反转`Inversion of Control` ，控制权从应用程序转移到框架（如IoC容器），是框架共有的特性。

- IoC(控制反转)容器：容器主动控制

  ```java
  //返回装配好的a
  A a = ApplicationContext.getBean("a");
  ```

  ```xml
  <!-- 配置文件 -->
  <bean id="a" class="AImpl">
  	<property name="b" ref="b"/>
  </bean>
  <bean id="b" class="BImpl"></bean>
  ```

- 本质：创建对象和装配对象、管理对象生命周期

  - 被动实例化，被动接受依赖，被动装配
  - （工厂+反射+xml）
  - 通用

  > IoC容器：实现了IoC思想的容器就是IoC容器

- IoC容器的特点：

  - 无需主动new对象；而是描述对象应该如何被创建即可
    - IoC容器会帮你创建，即被动实例化
  - 不需要主动装配对象之间的依赖关系，而是描述需要哪个服务（组件）
    - IoC容器会帮你装配（即负责将他们关联起来），被动接受装配；
  - 主动变被动，好莱坞法则
  - 迪米特法则（最少知识原则）：不知道依赖的具体实现，只知道需要提供某类服务的对象（面向抽象编程），松散耦合，一个对象应当对其他对象有尽可能少的了解，不和陌生人（实现）说话
  - IoC是一种让服务消费者不直接依赖于服务提供者的组件设计方式，是一种减少类与类之间依赖的设计原则

- 理解IoC容器问题关键：控制的那些方面被反转了？

  - 谁控制谁？为什么叫反转？
    - IoC容器控制，而以前是应用程序控制，所以叫反转
  - 控制什么？
    - 控制应用程序所需要的资源（对象、文件...）
  - 为什么控制？
    - 解耦组件之间的关系
  - 控制的那些方面被反转了？
    - 程序的控制权发生了反转：从应用程序转移到了IoC容器

  > 容器：提供组件运行环境，管理组件声明周期（不管组件如何创建的以及组件之间关系如何装配的）
  >
  > IoC容器不仅仅具有容器的功能，而且还具有一些其他特性---如依赖装配

#### DI(依赖注入)

- 依赖注入`Dependendy Injection `，用一个单独的对象（装配器）来装配对象之间的依赖关系

- 理解DI问题的关键

  - 谁依赖于谁？
    - 应用程序依赖于IoC容器
  - 为什么需要依赖？
    - 应用程序依赖于IoC容器装配类之间的关系
  - 依赖什么东西？
    - 依赖了IoC容器的装配功能
  - 谁注入谁？
    - IoC容器注入应用程序
  - 注入什么东西？
    - 注入应用程序需要的资源（类之间的关系）
  - IoC容器应该具有依赖注入功能，因此也叫DI容器

- DI优点：

  - 帮你看清组件之间的依赖关系，只需观察依赖注入的机制（setter/构造器），就可以掌握整个依赖（类与类之间的关系）

  - 组件之间的依赖关系有容器在运行期决定，形象的来说，即由容器动态的将某种依赖关系注入到组件之中

  - 依赖注入的目标并非为软件系统带来更多的功能，而是为了提升组件重用的概率，并为系统搭建一个灵活、可拓展的平台。通过依赖注入机制，我们只需要通过简单的配置，而无需任何代码就可指定目标需要的资源，完成自身的业务逻辑，而不用关心具体资源来自何处，由谁实现。

  - 使用DI限制：

    组件和装配器（IoC容器）之间不会有依赖关系，因此组件无法从装配器哪里获得更多服务，只能获得配置信息中所提供的那些

- 实现方式

  - 构造器注入
  - setter注入
  - 接口注入：在接口中定义需要注入的信息，并通过接口完成注入

#### 小结

- 使用IoC/DI容器开发需要改变的思路
  1. 应用程序不主动创建对象，但要描述创建他们的方式
  2. 在应用程序代码中不直接进行服务的装配，但要配置文件中描述哪一个组件需要哪一项服务。容器负责将这些装配在一起。
- 原理：基于OO设计原则的The Hollywood Principle：don't call us,we will call you.也就是说，所有的组件都是被动的（Passive），所有的组件初始化和装配都由容器负责。组件处在一个容器当中，由容器负责管理。
- IoC容器功能：
  - 实例化、初始化组件、装配组件依赖关系、负责组件生命周期管理。
- 本质：
  - IoC：控制权转移，由应用程序转移到框架；
  - IoC/DI容器：由应用程序主动实例化对象变被动等待对象（被动实例化）；
  - DI：由专门的装配器装配组件之间的关系
  - IoC/DI：由应用程序主动装配对象依赖变应用程序被动接受依赖





