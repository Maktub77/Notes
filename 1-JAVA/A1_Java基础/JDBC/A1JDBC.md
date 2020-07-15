### JDBC

![](C:\Users\Maktub\Documents\Notes\0-picture\A0_Photo\JDBC\02.png)

* JDBC(Java DataBase Connection)  Java数据库连接，可以通过Java语言（代码）连接到数据库（My SQL/Oracle/BD2...），从而达到数据化的持久化处理。

* SQL指令（DML、DDL、存储过程、触发器、函数、程序包）

#### JDBC步骤

![](C:\Users\Maktub\Documents\Notes\0-picture\A0_Photo\JDBC\JDBC.png)

1. 建立Java代码与数据库之间的连接

   1. 导包（驱动包：封装好的与数据库连接的Java代码）：可以帮助我们和数据库建立连接

   2. 将驱动包导入当前项目的过程（在当前项目中先新建一个文件夹-->将jar包粘贴到此文件夹中-->选中jar,右键-->Build Path --> Add to Build Path）

   3. 加载驱动中的驱动类（运用反射机制）

      ```java
      Class.forName("com.mysql.jdbc.Driver");
      ```

   4. 通过驱动管理器获取连接（驱动被加载后被驱动管理器DriverManager所管理）

      ```java
      conn = DriverManager.getConnection("jdbc:mysql://localhost/lianxi?useUnicode=true&characterEncoding=utf-8", "root", "123456");
      ```

      > 一个`Java.sql.Connection`类的对象，就是一个数据库连接，即conn就是Java和数据库的连接

2. 准备需要进行操作的SQL指令

   ```java
   String sql = "insert into users (uid,uname,password,uage)values(('"+uid+"', '"+uname+"', '"+password+"', '"+uage+"')";
   ```

3. 通过连接发送SQL语句

   ```java
   // 从连接conn中调用createStatement()方法获得一个Statement类的对象,这个对象可以加载/编译/执行sql语句
   sta = conn.createStatement();
   ```

4. 执行SQL语句获取执行结果

   ```java
   boolean flag = sta.execute(sql);
   if(!flag){
       System.out.println("添加成功");
   }else{
       System.out.println("添加失败");
   }
   
   int n = psta.executeUpdate(sql);
   if(n>0){
       System.out.println("添加成功");
   }else{
       System.out.println("添加失败");
   }
   ```

5. 关闭连接

   ```java
   if(sta !=null){
       sta.close();
   }
   if(conn != null){
       conn.close();
   }
   ```

#### Statement类

- SQL语句执行接口

- `Statement sta = conn.createStatement();`
- Statement是用来对要执行的SQL语句进行加载、编译/解析，并可以通过此对象调用方法：
  - `boolean execute(sql);`对应增删改SQL语句调用此方法，返回false表示执行成功；
  - `Int executeUpdate();`对应增删改SQL语句对应此方法，返回整数表示影响的行数
  - `ResultSet executeQuery(sql);`对于查询语句调用此方法，返回所有查询数据，存放在ResultSet对象中

#### PreparedStatement类

- 是Statement的子类，预编译的Statement

- 是对SQL语句进行预处理，解决SQL注入问题

  ```java
  PreparedStatement psta = conn.prepareStatement(sql);
  Psta.setString(index, value);
  Psta.setInt(index, value);
  Psta.setDouble(index, value);
  ```

- 执行SQL语句

  ```java
  Boolean execute();
  Int executeUpdate();
  ResultSet executeQuery();
  ```

#### sql注入问题

* 由于参数的改变导致sql语句原意发生改变

* 解决

  * sql语句中所有的参数不再使用字符串拼接，直接使用占位符"?"

  * 加载sql语句不再使用Statement对象，而使用Statement的一个子类`PerparedStatement`

  * 在通过`PreparedStatement`对对象执行Sql语句之前，调用set方法对sql语句中的"?"赋值

  * 在通过执行`PreparedStatement`对象执行sql语句时，不再需要知道sql参数

    ```java
    //1.加载驱动
    Class.forName("com.mysql.jdbc.Driver");
    
    //2.创建连接
    conn = DriverManager.getConnection("jdbc:mysql://localhost/lianxi?useUnicode=true&characterEncoding=utf-8", "root", "123456");
    
    //3.准备sql语句
    String sql = "insert into users (uid,uname,password,uage)values(?,?,?,?)";
    
    //4.通过连接加载sql指令
    psta = conn.prepareStatement(sql);
    psta.setInt(1, uid);
    psta.setString(2, uname);
    psta.setString(3, password);
    psta.setInt(4, uage);
    
    //5.通过PerparStatement对象，调用execute/executeUpdate()方法执行sql语句
    boolean flag = psta.execute();
    if(!flag){
        System.out.println("添加成功");
    }else{
        System.out.println("添加失败");
    }
    ```

#### ResultSet类

* 结果集操作接口

* 查询结果集接口，它对返回的结果集进行处理

  ```java
  public DeptDTO queryById(int id) {
      DeptDTO d = null;
      try {
          conn = DBManager.getConn();
          String sql = "select* from dept where dept_no=?";
          psta = conn.prepareStatement(sql);
          psta.setInt(1, id);
  
          rs = psta.executeQuery();
          if(rs.next()){
              String d_name = rs.getString("d_name");
              String d_addr = rs.getString("d_addr");
              d = new DeptDTO(id, d_name, d_addr);
          }	
      } catch (SQLException e) {
          e.printStackTrace();
      }
      return d;
  }
  
  public List<DeptDTO> listAll() {
      List<DeptDTO> list = new ArrayList<DeptDTO>();
      try {
          conn = DBManager.getConn();
          String sql = "select*from dept ";
          psta = conn.prepareStatement(sql);
  
          rs = psta.executeQuery();
          while(rs.next()){
              int dept_no = rs.getInt("dept_no");
              String d_name = rs.getString("d_name");
              String d_addr = rs.getString("d_addr");
              DeptDTO dd = new DeptDTO(dept_no, d_name, d_addr);
              list.add(dd);
          }
      } catch (SQLException e) {
          e.printStackTrace();
      }finally{
          DBManager.close(conn, psta, rs);
      }
      return list;
  }
  ```


#### 流程

1. 建表

2. 写DTO：数据的传输对象

3. 写DAO：数据访问接口

4. DAOImp：实现接口

5. service

6. servlet

 #### 批处理

* 批处理，只能执行增删改，不能执行查询

* Statement

  ```java
  Statement sta= conn.createStatement();
  sta.addBatch("insert into emps (e_name,e_age,dept_no)values ('zs',18,1001)");
  sta.addBatch("insert into emps (e_name,e_age,dept_no)values('ls',19,1002)");
  sta.addBatch("insert into emps (e_name,e_age,dept_no)values('ww',20,1002)");
  
  int [] arr = sta.executeBatch();
  ```

* PreparedStatement

  ```java
  PreparedStatement psta = conn.prepareStatement
  					("insert into emps(e_name,e_age,dept_no)values (?,?,?)");
  
  for (int i = 0; i < 10; i++) {
      psta.setString(1, "qq"+i);
      psta.setInt(2, 18+i);
      psta.setInt(3, 1001);
      //要想把以上数据批处理添加到数据库中，首先需要把数据添加到批处理里面
      psta.addBatch();
  }
  
  psta.executeBatch();
  ```

#### 存储过程

* CallableStatement是PreparedStatement的子类，用于处理存储过程sql语句

* ```java
  //调用存储过程的sql语句
  String sql = "{call p_a()}";	
  CallableStatement csta = conn.prepareCall(sql);	
  ```

* 判断存储过程中的sql语句，通过`csta.getResultSet()`方法拿到第一张虚拟表，通过`csta.getMoreResults()`方法指针指向下一个内容（虚拟表），获取更多的内容

  ```java
  if(csta.execute()){		//有可能其中的sql语句为增删改查，返回true
      //只是拿出存储过程里面第一张虚拟表	
      rs = csta.getResultSet();	
      while(rs.next()){
          System.out.print(rs.getString(1));	//获取表中该列的信息
      }
      //csta.getMoreResults()指针指向下一个内容（虚拟表），获取更多内容
      while(csta.getMoreResults()){
          rs = csta.getResultSet();
          while(rs.next()){
              System.out.println(rs.getString(2));
          }			
      }				
  }	
  ```

* 存储过程带参

  * `csta.registerOutParameter(2, java.sql.Types.INTEGER);`给带出的参数注册（注册数据类型）

  ```java
  //两个参数，一个管带入，一个管带出
  try {
      Connection conn = DBManager.getConn();
  
      String sql = "call p_e(?,?)";
  
      CallableStatement csta = conn.prepareCall(sql);
  
      csta.setInt(1, 1213);		//给第一个？赋值
  
      //给第个？注册
      csta.registerOutParameter(2, java.sql.Types.INTEGER);
  
      csta.execute();
      System.out.println( csta.getInt(2) );
  
      csta.close();
      conn.close();
  } catch (Exception e) {
  }
  ```

#### 事务(transaction)

* 事务是具有以下4个特征（ACID）的工作单元

  * 原子性（Atomicity，不可分割性）：原子操作，要么全执行，要么完全不执行
  * 一致性（Consistency）：事务执行前后数据库处于一致状态
  * 分离性（Isolation，独立性）：并发事务相互隔离
  * 持久性（Durability）：一旦一个事务提交DBMS保证对数据的改变是永久的，通过数据库备份和恢复来保证

  ```java
  public static void main(String[] args) {
      Connection conn = null;
      Statement sta = null;
      Savepoint sp = null;
  
      try {
          conn = DBManager.getConn();
          sta = conn.createStatement();
          //将事务的提交改为手动模式，默认ture自动提交
          conn.setAutoCommit(false);	
  
          String sql1 = "update people set money = money-1000 where p_id=1";
          String sql2 = "update people set money = money+1000 where p_id=2";
  
          sta.executeUpdate(sql1);
  //	    sp = conn.setSavepoint();	//指定回滚的位置 
          sta.executeUpdate(sql2);
  
      } catch (SQLException e) {
          try {
              conn.rollback(/*sp*/);	//回滚到指定位置
          } catch (SQLException e1) {
              e1.printStackTrace();
          }
          e.printStackTrace();
      }finally{
          try {
              conn.commit();		//提交事务
              if(sta!=null){
                  sta.close();
              }
              if(conn!=null){
                  conn.close();
              }
          } catch (SQLException e) {
              e.printStackTrace();
          }
      }	
  }
  ```

##### 回滚(rollback)

* 调用`rollback();`方法

* 定义回滚的位置

  ```java
  Savepoint sp = conn.setSavepoint();	//指定回滚的位置
  
  conn.rollback(sp);	//回滚到指定位置
  ```

##### 事务的隔离级别

* JDBC事务并发产生的问题

  * **脏读(Dirty Reads)**：一个事务读取了另一个并行事务还未提交的数据
  * **不可重复读(UnRepeatable Read)**：一个事务再次读取之前的数据时，得到的数据不一致，被另一个已提交的事务修改
  * **幻读(Phantom Read)**：一个事务重新执行一个查询，返回的记录中包含了因为其他最近提交的事务而产生的新记录

  为避免以上问题的出现，则采用

* 事务隔离级别：

  | TRANSACTION_NONE             | 不使用事务                                                   | 0级  |
  | ---------------------------- | ------------------------------------------------------------ | ---- |
  | TRANSACTION_READ_UNCOMMITTED | 可以读取未提交数据                                           | 1级  |
  | TRANSACTION_READ_COMMITTED   | 可以避免脏读，不能够读取没提交的数据                         | 2级  |
  | TRANSACTION_REPEATABLE_READ  | 可以避免脏读，不可以重复读取， 最常用的隔离级别 大部分数据库的**默认隔离级别** | 3级  |
  | TRANSACTION_SERIALIZABLE     | 可以避免脏读，不可重复读取和幻读， （事 务串行化）会降低数据库效率 | 4级  |

  * 以上五个事务隔离级别都是在Connection类中定义的静态常量，使用`setTransactionIsolation(int level) `方法可以设置事务隔离级别。

    ```java
    conn.setTransactionIsolation(conn.TRANSACTION_READ_COMMITTED);
    ```


