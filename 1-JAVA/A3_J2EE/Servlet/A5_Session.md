### Session

* 域对象
* 同一会话下，可以使一个应用的多个资源共享数据
* cookie客户端技术，只能存字符串。HttpSession服务端的技术，它可以存对象

#### 常用方法

* 把数据保存在HttpSession对象中，该对象也是一个域对象
  * void setAttribute(String name,Object value);
  * Object getAttribute(String name);
  * void removeAttribute(String name);
  * HttpSession.getId();

* `setMaxInactiveInterval(int interval)`设置session存活时间

* `web.xml`中：设置session时间，以分钟为单位

  ```xml
  <session-config>
  	<session-timeout>1</session-timeout>
  </session-config>
  ```

* invalidate() 使此会话无效

#### getSession()

* 内部执行原理
  * 获取名称为JESSIONID的cookie的值

  * 没有这样的的cookie,创建一个新的HttpSession对象，分配一个唯一的SessionID，并且向客户端写了一个名字为JESSIONID=sessionID的cookie

  * 有这样的Cookie,获取cookie的值（即HttpSession对象的值），从服务器的内存中根据ID找那个HttpSession

    * 找到了：取出继续为你服务

    * 找不到：从2开始

      HttpSession request.getSession(boolean create);

      ​	参数：

      ​	true：和getSession()功能一样

      ​	false：根据客户端JSESSIONID的cookie的值，找对应的HttpSession对象，找不到返回null(不会创建新的，只是查询)

### Cookie

* 由于Cookie数据是由客户端来保存和携带的，所以称之为客户端技术。

  