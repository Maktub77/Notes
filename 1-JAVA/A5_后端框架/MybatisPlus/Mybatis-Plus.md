## Mybatis-Plus(MP)

https://www.jianshu.com/p/df543044e8e2

* 是一个Mybatis的增强工具，在Mybatis的基础上只做增强不做改变，为简化开发、提高生产效率而生。

### 问题

* mybatis-plus怎么实现单表URUD操作？
* mybatis-plus的底层实现原理是什么？
* mybatis-plus与其他同类框架如mybatis helper有很什么优势？
* 如何集成mybatis-plus快速搭建一个spring boot项目。

### 特性：

![img](https://upload-images.jianshu.io/upload_images/4120002-2db6a84c242282f1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/863)

### 常用实体注解:

## ![img](https://upload-images.jianshu.io/upload_images/4120002-49c775723aca1b6d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/509)

其中实体无注解化设置可以如下处理：

- 当数据库的表字段名是驼峰命名时无需注解处理。
- 或者全局配置： 下划线命名 dbColumnUnderline 设置 true , 大写 isCapitalMode 设置 true

###　简化 CURD

![img](https://upload-images.jianshu.io/upload_images/4120002-f61ea8a51d371583.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/698)

* ActiveRecord的模式写法，不用注入userMapper，new了一个对象之后直接调用方法操作就行了。

* 复杂的查询，新建一个EntityWrapper作为查询对象，Wrapper接口封装了很多常用的方法。几乎sql能写出来的条件调用Wrapper的方法就能表现出来。

###　架构原理

［[mybatis-plus 实践及架构原理.pdf](https://gitee.com/baomidou/mybatis-plus/attach_files)

### mybatis plus代码生成器与 mybatis generator

* **velocity**

![img](https://upload-images.jianshu.io/upload_images/4120002-1bb303902ad0c570.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000)

而mybatis generator生成的代码就是基本的增删改查和实体。模板好像改不了，灵活性明显不够。

### mp插件拓展

![img](https://upload-images.jianshu.io/upload_images/4120002-4118134a4da7dbb5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/694)

#### 分页插件

1. 自定义查询语句分页（自己写sql/mapper）

   spring 注入 mybatis 配置分页插件

   ```xml
   <plugins> 
       <!-- 
           分页插件配置 
           插件提供二种方言选择：1、默认方言 2、自定义方言实现类，两者均未配置则抛出异常！ 
           overflowCurrent 溢出总页数，设置第一页 默认false 
           optimizeType Count优化方式 （ 版本 2.0.9 改为使用 jsqlparser 不需要配置 ） 
        --> 
       <!-- 注意!! 如果要支持二级缓存分页使用类 CachePaginationInterceptor 默认、建议如下！！ --> 
       <plugin interceptor="com.baomidou.mybatisplus.plugins.PaginationInterceptor"> 
           <property name="sqlParser" ref="自定义解析类、可以没有" /> 
           <property name="localPage" value="默认 false 改为 true 开启了 pageHeper 支持、可以没有" /> 
           <property name="dialectClazz" value="自定义方言类、可以没有" /> 
       </plugin> 
   </plugins>
   ```

   ```java
   //Spring boot方式 
   @EnableTransactionManagement @Configuration @MapperScan("com.baomidou.cloud.service.*.mapper*") 
   public class MybatisPlusConfig { 
       /** * 分页插件 */
       @Bean 
       public PaginationInterceptor paginationInterceptor() { 
           return new PaginationInterceptor(); 
       } 
   }
   ```

   * UserMapper.java

     ```java
     public interface UserMapper{
         //可以继承或者不继承BaseMapper 
         /** 
         * <p> 
         * 查询 : 根据state状态查询用户列表，分页显示 
         * </p> 
         * 
         * @param page 
         * 翻页对象，可以作为 xml 参数直接使用，传递参数 Page 即自动分页 
         * @param state 
         * 状态 
         * @return 
         */
         List<User> selectUserList(Pagination page, Integer state); 
     }
     ```

   * UserServiceImpl.java 调用分页方法，需要 page.setRecords 回传给页面

     ```java
     public Page<User> selectUserPage(Page<User> page, Integer state) {
         return page.setRecords(userMapper.selectUserList(page, state));
     }
     ```

   * UserMapper.xml 等同于编写一个普通 list 查询，mybatis-plus 自动替你分页

     ```xml
     <select id="selectUserList" resultType="User">
         SELECT * FROM user WHERE state=#{state}
     </select>
     ```
