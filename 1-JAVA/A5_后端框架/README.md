### ORM

- (Object/Relation Mapping) 对象-关系数据库映射
- 解决面向对象与关系数据库存在互不相通的技术。
- dao层调用save方法可以直接将对象持久化到数据库(ORM框架的作用 -- 数据持久化)

#### 常见框架

* SSM

  * Mybatis
  * spring
  * springMVC

* SSH
  * Struts2
  * spring
  * Hibernate

* ORM(Object/Relation Mapping) 对象-关系数据库映射
  * Mybatis (轻量级)
    * 是一个SQL语句映射的框架(工具)
    * 注重pojo与SQL之间的映射关系，不会为程序员在运行期自动生成SQL
    * 自动化程度低、手工映射SQL，灵活程度高
    * 需要开发人员熟练掌握SQL语句

  * Hibernate(重量级)

    * 主流的ORM框架、提供了从POJO到数据库表的全套映射机制
    * 会自动生成全套SQL语句
    * 因为自动化程度高、映射配置复杂，api(Application Programming Interface\应用程序编程接口)也相对复杂，灵活性低
    * 开发人员不必关注SQL底层语句开发

    ![](E:\笔记\A0_Photo\持久层框架.bmp)

* MVC -- springMVC

* dao层调用save方法可以直接将对象持久化到数据库(ORM框架的作用 -- 数据持久化)

