1. pom.xml中指定工程所依赖jar包

2. jdbc.properties中指定数据库的连接方式

3. MyBatis相关配置

4. Spring相关配置

   - **spring-dao**（jdbc/mybatis）
     1. 定义需要读取变量配置文件的位置
     2. 数据库连接池
     3. 用来创建数据库连接池的对象
     4. 配置扫描(DAO)路径,传入自动创建的连接池的数据

   - **spring-service**(事物管理)

     1. 使用注解类型

     2. 配置事物管理性

   - **spring-web**(配置springMVC)

     1. 开启springMVC注解模式

     2. 静态资源默认servlet配置

     3. 定义视图解析器：根据url请求解析jsp/html文件

     4. 扫描web相关的bean

5. web.xml关联配置

   ```xml
   <servlet>
   	<servlet-name>spring-dispatcher</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <init-param>
       	<param-name>contextConfigLocation</param-name>
           <param-value>classpath:spring/spring-*.xml</param-value>
       </init-param>
   </servlet>
   <servlet-mapping>
   	<servlet-name>spring-dispatcher</servlet-name>
       <!-- 默认匹配所有的请求 -->
       <url-pattern>/</url-pattern>
   </servlet-mapping>
   ```



配置spring和junit整合，junit启动时加载springIOC容器

```java
@RunWith(SpringJUnit4ClassRunner.class)
//告诉junit spring配置文件的位置
@ContextConfigration({"classpath:spring/spring-dao.xml"})
public class BaseTest{
    
}
```



