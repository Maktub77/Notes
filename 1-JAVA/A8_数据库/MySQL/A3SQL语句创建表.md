###    SQL语句创建表

* **关键字：create**

* create table 表名（             

  ​	字段名1 数据类型 auto_increment,     //自动递增

  ​	字段名2 数据类型 not null,                   //不为空

  ​	字段名3 varchar (长度) default '...',     // 默认'....'

  ​	... ...

  ​	primary key(字段名1),    //设置主键

  ​	Constraint 外键名 Foreign key(当前表字段名) 			references 表名 (表主键字段名)

  ）;

```mysql
Create table users(
    uid int auto_increment,
    uname varchar(20) not null,
    password varchar(20) not null,
    uage int default 18,
    primary key(uid)
);
```

### 添加数据

* **增 	关键字 :insert**

* **insert into 表名 values（值1,值2,....）;**

  ```mysql
  insert into users values(1,'张三','zs123456',22);
  ```

* **insert into 表名（字段名1，字段名2...）values(值1，值2...);**

  ```mysql
  insert into users(uid,uname,password,uage)values(10,'李四','ls123',23);
  insert into users(uname,password)values('ww','ww123');
  ```

  > 以最大主键为基础自动递增

* 把一个表里面的数据添加到另一个表中（前提条件，相关表的字段及字段类型必须一样）

  ```mysql
  Insert into users(uid,uname,password,uage)select 
  	sid,sname,sename,sage from t_student where
  		sscore>500 and ssex='女';
  ```

### 更新数据

* **改    关键字：update**

* **update 表名 set 字段=新值 where \*\*\*=值（条件）;** 

  ```mysql
  update users set uname='ccc' where uid=10;
  ```

  * 把学生表中600分以上的性别改为'男'

    ```mysql
    update t_student set ssex='男' where sscore>600;
    ```

### 删除数据

* **删     关键字：delete**

* **delete from 表名 where 属性=值;** 

  * 删除学生表中的所有女生

    ```mysql
    delete from t_student where ssex='女';
    ```

    

