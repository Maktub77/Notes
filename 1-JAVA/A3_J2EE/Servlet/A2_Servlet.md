###  Servlet

* 特殊的Java类，这个类必须继承HttpServlet
* 响应客户端请求的4个方法
  * doGet：用于响应客户端的GET请求
  * doPost：用于响应客户端的POST请求
  * doPut：用于响应客户端的PUT请求
  * doDelete：用于响应客户端的DELETE请求
* HttpServlet包含两种方法
  * init(ServletConfig config)：创建Servlet实例时，调用该方法初始化Servlet资源
  * destroy()：销毁Servlet实例时，自动调用该方法回收资源

#### 生命周期

* 客户端 ---(请求)--- 服务端 ---(加载xml文件)

* xml文件

  * 访问路径 ---(通过相同servlet-name) --- 找到类文件--执行
  * 类文件（初始化---执行---销毁）init(初始化)--构造函数-service(处理请求)--destory(结束)
  * `<servlet>` ---映射：通过反射--- `<servlet-mapping>`

  ```xml
  <servlet>
      <servlet-name>HelloServlet</servlet-name>
      <servlet-class>com.servlet.HelloServlet</servlet-class> <!-- 类名 -->
  </servlet>
  
  <servlet-mapping>
      <servlet-name>HelloServlet</servlet-name>
      <url-pattern>/HelloServlet</url-pattern>	<!-- 访问路径 -->
  </servlet-mapping>
  ```

  ```jsp
  <load-on-startup>1</load-on-startup> <!-- 设置优先级 -->
  ```

#### 线程

```java
private int sum = 0;

protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    //设置锁
    synchronized (response) {
        sum++;
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(Thread.currentThread().getName()+"修改了结果！");
    }
    response.getWriter().write(sum+"");
}
```

#### ServletConfig/ServletContext

* ServletConfig

  * 获取当前Servlet配置的初始化参数（web.xml中）
  * 当Servlet配置了初始化参数后，web容器在创建Servlet实例对象时，会自动将这些初始化参数封装到ServletConfig对象中，并调用Servlet的init方法时，将ServletConfig对象传递给Servlet。
  	 进而，程序员通过ServletConfig对象就可以得到当前Servlet的初始化参数信息	

* ServletContext

  * 配置全局参数

    ```xml
    <context-param>
        <param-name>encode</param-name>
        <param-value>GBK</param-value>
    </context-param>
    ```

    并在Servlet中获取参数值

    ```java
    String encode2 = this.getServletContext().getInitParameter("encode");
    System.out.println(encode2);
    ```

  * 加载资源文件

    加载属性文件

    ```java
    InputStream is = 当前类.class.getClassLoader().getResourceAsStream("a.properties");
    Properties p = new Properties();
    p.load(is);
    p.list(System.out);
    ```

#### request

1. 获取客户端的信息

2. 获取客户端的头信息

3. 页面间传递参数

   ```jsp
   String 参数值 = request.getParament(参数名)；
   ```

4. 页面间的跳转（服务器跳转）

   ```jsp
   request.getRequestDispatcher("跳转的路径").forward(request,response);
   ```

   >注意路径： 绝对路径  相对路径

5. 放置参数

   ```jsp
   request.setAttribute(String key,value);
   ```

   

