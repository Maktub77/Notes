## MyBatis

* MyBatis是一种半自动化的ORM实现，是一个轻量级的持久层框架

* MyBatis是一个优秀的持久层框架，它对jdbc的操作数据库进行封装。

* MyBatis通过xml或注解的方式将要执行的各种statement(`statement、preparedStatemnt、CallableStatement`)配置起来，并通过Java对象和statement中的sql映射生成最终执行的sql语句，最后又MyBatis框架执行sql并将结果集映射成Java对象并返回

* MyBatis架构

  ![](C:\Users\Maktub\Documents\Notes\0-picture\A0_Photo\MyBatis架构.png)

  1. MyBatis配置
     * SqlMapConfig.xml，此文件作为MyBatis的全局配置文件，配置了MyBatis的运行环境等信息。

       ```xml
       <?xml version="1.0" encoding="UTF-8"?>
       <!DOCTYPE configuration
       PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
       "http://mybatis.org/dtd/mybatis-3-config.dtd">
       <configuration>
           <!-- 和spring整合后 environments配置将废除 -->
           <environments default="development">
               <environment id="development">
                   <!-- 使用jdbc事务管理 -->
                   <transactionManager type="JDBC" />
                   <!-- 数据库连接池 -->
                   <dataSource type="POOLED">
                       <property name="driver" value="com.mysql.jdbc.Driver" />
                       <property name="url"
                                 value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8" />
                       <property name="username" value="root" />
                       <property name="password" value="123456" />
                   </dataSource>
               </environment>
           </environments>
           <-- 加载映射文件 -->
           <mappers>
               <mapper resource="User.xml"></mapper>
               <mapper resource="Order.xml"></mapper>
           </mappers>
       </configuration>
       ```

     * mapper.xml文件即sql映射文件，文件中配置了操作数据库的sql语句。此文件需要在SqlMapConfig.xml中加载

       ```java
       Public class User {
           private int id;
           private String username;// 用户姓名
           private String sex;// 性别
           private Date birthday;// 生日
           private String address;// 地址
       getter/setter……
       ```

       ```xml
       <?xml version="1.0" encoding="UTF-8" ?>
       <!DOCTYPE mapper
       PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
       "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
       <!-- namespace：命名空间，用于隔离sql -->
       <!-- id:statement的id 或者叫做sql的id-->
       <!-- parameterType:声明输入参数的类型 -->
       <!-- resultType:声明输出结果的类型，应该填写pojo的全路径 -->
       <!-- #{}：输入参数的占位符，相当于jdbc的？ -->
       <mapper namespace="User">
           <select id="selectUserLike" resultType="com.softeem.mybatis.po.User" parameterType="String">
               <!-- select * from user where username like "%"#{value}"%"; -->
               select * from user where username like '%${value}%';
           </select>
       
           <update id="updateUserById" parameterType="com.softeem.mybatis.po.User" >
               update user set username=#{username},sex=#{sex},birthday=#{birthday},address=#{address} where id=#{id};
           </update>
       </mapper>
       ```

  2. 通过MyBatis环境等配置信息构造SQLSessionFactory即会话工厂

     ```java
     InputStream is = Resources.getResourceAsStream("SqlMapConfig.xml");
     SqlSessionFactory ssf =  new SqlSessionFactoryBuilder().build(is);
     ```

  3. 由会话工厂创建sqlSession即会话，操作数据库需要通过sqlSession进行

     ```java
     SqlSession ss = ssf.openSession();
     ```

     ```java
     User user = ss.selectOne("User.selectUserById", 1);
     List<User> list = ss.selectList("User.selectUserLike", "小");
     
     //增删改需要提交事务
     ss.insert("User.addUser", user);
     ss.delete("User.deleteUser", 27);
     ss.update("User.updateUserById", user);
     //事务提交
     ss.commit();
     ```

  4. MyBatis底层自定义了Executor执行器接口操作数据库，Executor接口有两个实现，一个是基本执行器，一个是缓存执行器

  5. MappedStatement 也是MyBatis一个底层封装对象，它包装了MyBatis配置信息及sql映射信息等。mapper.xml文件中一个sql对应一个MappedStatement对象，sql的id即是MappedStatement的id

  6. MappedStatement 对sql执行输入参数进行定义，包括HashMap、基本类型、pojo,Executor通过MappedStatement
     * 在执行sql前将输入的Java对象映射值sql中，输入参数映射就是jdbc编程中对perparedStatement设置参数
     * 在执行sql后将输出结果映射值Java对象中，输出结果映射过程相当于jdbc编程过程中对结果的解析处理过程

### 小结

#### #{}和${}

* \#{}表示一个占位符号，通过#{}可以实现preparedStatement向占位符中设置值，自动进行java类型和jdbc类型转换。#{}可以有效防止sql注入。 #{}可以接收简单类型值或pojo属性值。 如果parameterType传输单个简单类型值，#{}括号中可以是value或其它名称。

* \${}表示拼接sql串，通过\${}可以将parameterType 传入的内容拼接在sql中且不进行jdbc类型转换， \${}可以接收简单类型值或pojo属性值，如果parameterType传输单个简单类型值，${}括号中只能是value。

#### parameterType和resultType

* parameterType：指定输入参数类型，mybatis通过ognl从输入对象中获取参数值拼接在sql中。
* resultType：指定输出结果类型，mybatis将sql查询结果的一行记录数据映射为resultType指定类型的对象。如果有多条数据，则分别进行映射，并把对象放到容器List中

#### mysql自增主键返回

* 查询id的sql

  SELECT LAST_INSERT_ID()

  ```xml
  <!-- 保存用户 -->
  <insert id="saveUser" parameterType="com.softeem.mybatis.pojo.User">
      <!-- selectKey 标签实现主键返回 -->
      <!-- keyColumn:主键对应的表中的哪一列 -->
      <!-- keyProperty：主键对应的pojo中的哪一个属性 -->
      <!-- order：设置在执行insert语句前执行查询id的sql，还是在执行insert语句之后执行查询id的sql -->
      <!-- resultType：设置返回的id的类型 -->
      <selectKey keyColumn="id" keyProperty="id" order="AFTER"
                 resultType="int">
          SELECT LAST_INSERT_ID()
      </selectKey>
      INSERT INTO `user`
      (username,birthday,sex,address) VALUES
      (#{username},#{birthday},#{sex},#{address})
  </insert>
  ```

#### mybatis与hibernate

* mybatis：MyBatis需要程序员自己编写Sql语句，无法做到数据库无关性。
* Hibernate：对象/关系映射能力强，数据库无关性好。

### Mapper动态代理方式

#### 开发规范

- Mapper接口开发方法只需要程序员编写Mapper接口（相当于Dao接口），由Mybatis框架根据接口定义创建接口的动态代理对象，代理对象的方法体相当于Dao接口方法体
- Mapper接口开发需要遵循以下规范：
  1. Mapper.xml文件中的namespace与mapper接口的类路径相同。
  2. Mapper接口方法名和Mapper.xml中定义的每个statement的id相同
  3. Mapper接口方法的输入参数类型和mapper.xml中定义的每个sql 的parameterType的类型相同。
  4. Mapper接口方法的输出参数类型和mapper.xml中定义的每个sql的resultType的类型相同

#### Mapper.xml

- UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!-- namespace：命名空间，用于隔离sql -->
<!-- 还有一个很重要的作用，使用动态代理开发DAO，1. namespace必须和Mapper接口类路径一致 -->
<mapper namespace="com.softeem.mybatis.mapper.UserMapper">
	<!-- 根据用户id查询用户 -->
	<!-- 2. id必须和Mapper接口方法名一致 -->
	<!-- 3. parameterType必须和接口方法参数类型一致 -->
	<!-- 4. resultType必须和接口方法返回值类型一致 -->
	<select id="queryUserById" parameterType="int"
		resultType="com.softeem.mybatis.pojo.User">
		select * from user where id = #{id}
	</select>
</mapper>
```

#### Mapper(接口文件)

- UserMapper

```java
public interface UserMapper {
	/**
	 * 根据id查询
	 */
    public User queryUserById(int id);
}
```

#### 加载UserMapper.xml文件

- 修改SqlMapConfig.xml文件

```xml
<!-- 加载映射文件 -->
	<mappers>
		<mapper resource="mapper/UserMapper.xml" />
	</mappers>
```

#### 测试

```java
public class UserMapperTest {
    private SqlSessionFactory sqlSessionFactory;

    @Before
    public void init() throws Exception {
        // 创建SqlSessionFactoryBuilder
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        // 加载SqlMapConfig.xml配置文件
        InputStream inputStream = Resources.getResourceAsStream("SqlMapConfig.xml");
        // 创建SqlsessionFactory
        this.sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
    }

    @Test
    public void testQueryUserById() {
        // 获取sqlSession，和spring整合后由spring管理
        SqlSession sqlSession = this.sqlSessionFactory.openSession();

        // 从sqlSession中获取Mapper接口的代理对象
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // 执行查询方法
        User user = userMapper.queryUserById(1);
        System.out.println(user);

        // 和spring整合后由spring管理
        sqlSession.close();
    }
}
```

> namespace官方推荐使用mapper代理开发mapper接口，程序员不用编写mapper接口实现类，使用mapper代理方法时，输入参数可以使用pojo包装对象或map对象，保证dao的通用型

### SqlMapConfig.xml配置文件

- SqlMapConfig.xml中配置的内容和顺序如下：

  1. **properties**(属性)

     - SqlMapConfig.xml可以引用java属性文件中的配置信息

       - db.properties配置文件

         ```properties
         jdbc.driver=com.mysql.jdbc.Driver
         jdbc.url=jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8
         jdbc.username=root
         jdbc.password=root
         ```

       - SqlMapConfig.xml引用

         ```xml
         <!-- 是用resource属性加载外部配置文件 -->
         <properties resource="db.properties">
             <!-- 在properties内部用property定义属性 -->
             <!-- 如果外部配置文件有该属性，则内部定义属性被外部属性覆盖 -->
             <property name="jdbc.username" value="root123" />
             <property name="jdbc.password" value="root123" />
         </properties>
         <!-- 和spring整合后 environments配置将废除 -->
         <environments default="development">
             <environment id="development">
                 <!-- 使用jdbc事务管理 -->
                 <transactionManager type="JDBC" />
                 <!-- 数据库连接池 -->
                 <dataSource type="POOLED">
                     <property name="driver" value="${jdbc.driver}" />
                     <property name="url" value="${jdbc.url}" />
                     <property name="username" value="${jdbc.username}" />
                     <property name="password" value="${jdbc.password}" />
                 </dataSource>
             </environment>
         </environments>
         ```

         > 注： MyBatis 按照下面的顺序来加载属性：
         >
         > 1. 在 properties 元素体内定义的属性首先被读取
         >    1. u 然后会读取properties 元素中resource或 url 加载的属性，它会覆盖已读取的同名属性。 

  2. settings(全局配置参数)

  3. **typeAliases**(类型别名)

     - mybatis支持别名

       | 别名       | 映射的类型 |
       | ---------- | ---------- |
       | _byte      | byte       |
       | _long      | long       |
       | _short     | short      |
       | _int       | int        |
       | _integer   | int        |
       | _double    | double     |
       | _float     | float      |
       | _boolean   | boolean    |
       | string     | String     |
       | byte       | Byte       |
       | long       | Long       |
       | short      | Short      |
       | int        | Integer    |
       | integer    | Integer    |
       | double     | Double     |
       | float      | Float      |
       | boolean    | Boolean    |
       | date       | Date       |
       | decimal    | BigDecimal |
       | bigdecimal | BigDecimal |
       | map        | Map        |

     - 自定义别名

       ```xml
       <typeAliases>
           <!-- 单个别名定义 -->
           <typeAlias alias="user" type="com.softeem.mybatis.pojo.User" />
           <!-- 批量别名定义，扫描整个包下的类，别名为类名（大小写不敏感） -->
           <package name="com.softeem.mybatis.pojo" />
           <package name="其它包" />
       </typeAliases>
       ```

       - 在mapper.xml配置文件中就可以使用设置的别名了

         ```xml
         <select id="queryUserById" parameterType="int"
                 resultType="User">
             SELECT * FROM `tb_user` WHERE id  = #{id}
         </select>
         ```

         > 别名大小写不铭感

  4. typeHandlers(类型处理器)

  5. objectFactory(对象工厂)

  6. plugins(插件)

  7. environments（环境集合属性对象）

     - environment(环境子属性对象)
       - transactionManager(事物管理)
         - JDBC：使用JDBC的默认事务管理配置
         - MANAGED：放弃事务管理(由其他事务管理器管理：如spring管理-AOP)
       - dataSource（数据源：连接池）
         - UNPOOL：不使用连接池
         - POOLED：使用mybatis提供的默认连接池
         - JNDI：(自定义连接池) 一般用于web项目中，主要通过web容器(tomcat,jboos)中的配置获取数据

  8. **mappers**(映射器)

     - 配置的几种方法

     - 配置映射文件(xml文件)：'/' : 资源路径；'.' : 类路径

     - <mapper resource=""/>

       - 使用相对于类路径的资源

         如：<mapper resource="sqlmap/User.xml" />

     - <mapper class=""/>

       - 使用mapper接口类路径

         如：<mapper class="com.softeem.DAO.UserMapper"/>

         > 此种方法要求mapper接口名称和mapper映射文件名称相同，且放在同一个目录中

     - <package name =""/>

       - 注册指定包下的所有mapper接口

         如：<package name="com.softeem.DAO"/>

         > 此种方法要求mapper接口名称和mapper映射文件名称相同，且放在同一个目录中

### mybatis核心对象

- 每 一 个 MyBatis 的 应 用 程 序 都 以 一 个 `SqlSessionFactory `对 象 的 实 例 为 核 心

- 有了SqlSessionFactory对象，就可以 获得 `SqlSession `的实例了。 `SqlSession` 对象完全包含以数据库为背景的所有执行 SQL 操作的 方法。你可以用` SqlSession `实例来直接执行已映射的 SQL 语句 

  ```java
  //读取配置
  String resource = "mybatis-config.xml";
  //加载指定的配置文件为一个输入流
  InputStream is = Resources.getResourceAsStream(resource);
  //获取一个sqlSessionFactory对象
  SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(is);
  //获取session对象
  SqlSession session = factory.openSession();
  ```

#### 范围和生命周期

- `SqlSessionFactoryBuilder`：本地方法变量，只用来创建`SqlSessionFactory`

- `SqlSessionFactory`：最佳实践是在应用运行期间不要重复创建多次，最佳范围是应用范围(使用单例模式)

- `SqlSession`：`SqlSession` 的实例不能被共享,也是线程不安全的。因此最佳的范围是请求或方法范围。

  基于收到的 HTTP 请求,你可以打开 了一个 `SqlSession`,然后返回响应,就可以关闭它了。关闭 `SqlSession`很重要,你应该确保使 用 finally 块来关闭它。

### 动态sql

- if
- bind
- choose -- when -- otherwise
- where
- set
- foreach

### 关联查询

* 一对一查询
* 一对多查询

### MyBatis的缓存技术

* 缓存技术是一种“以空间换时间”的设计理念，利用内存空间资源来提高数据检索速度的有效手段之一。





