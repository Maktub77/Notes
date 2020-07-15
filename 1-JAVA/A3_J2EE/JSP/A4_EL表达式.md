### EL表达式

* Expression Language 表达式语言

* 一种用来简化java脚本的数据访问语言

* 语法：`${表达式}`

* EL 支持运算：${1*3=2}
    	<      lt

   	\>  	gt

  ​	>=  	ge
  	<= 	le
  	!= 	ne
  	== 	eq
  支持：三目运算符

* EL支持处理对象

  * `${param.key }`获取请求参数
  * `${requestScope.key }`获取request属性范围中的取值
  * `${sessionScope.key }`获取session属性范围中的取值
  *  `${applicationScope.key }`获取application属性范围中的取值
  * `${header }` 获取页面的头部信息

#### 服务器获取传递的对象

* JSP页面中可以通过EL表达式获取值

* Servlet服务器

  ```java
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
  
      request.setCharacterEncoding("utf-8"); //请求时编码方式的处理
      response.setCharacterEncoding("utf-8");//响应时的编码方式处理
  	
      //在request中存值
      request.setAttribute("key", "key_val");
  
  	//在session中存值
      HttpSession session = request.getSession();
      session.setAttribute("key", "sess_val");
  
      //在appliction中存值
      ServletContext application = request.getServletContext();
      application.setAttribute("key","app_val");
  
      //创建list对象
      List<Phone> list = new ArrayList<Phone>();
      list.add( new Phone("huawei", 4998, "白色", 7.5) );
      list.add( new Phone("vivo", 4998, "白色", 7.5) );	
      request.setAttribute("list", list);
  
      //创建map集合对象
      Map<String, Phone> map = new HashMap<String,Phone>();
      map.put("p1", new Phone("mi", 4998, "白色", 7.5));
      map.put("p2", new Phone("T", 4998, "白色", 7.5));
      request.setAttribute("map", map);
  
      //创建数组对象
      int [] arr = {1,2,3,4,5};
      request.setAttribute("arr", arr);
  
      request.setAttribute("sex", 1);
  
      //服务器跳转
      request.getRequestDispatcher("show.jsp?tid=110").forward(request, response);
  }
  ```

* JSP页面

  ```jsp
  <!-- 分别接收各个作用域中的值  -->
  <%
      String msg1 = (String)request.getAttribute("req_key");
      String msg2 = (String)session.getAttribute("sess_key");
      String msg3 = (String)application.getAttribute("app_key");		
  %>
  
  <!-- 输出到页面 -->
  <%=msg1 %> <br/>
  <%=msg2 %> <br/>
  <%=msg3 %> <br/>
  
  <h2>EL表达式获取四个作用域的值</h2>
  <!--  
      1.当四个作用域的key相同的时候，默认得到的结果
      request > session > application
  -->
  <%-- 
      ${req_key }<br/>
      ${sess_key }<br/>
      ${app_key }<br/> 
  --%>
  
  ${key }	<br/>
  <!--
      2.任然需要分别得到不同的值，则以'***Scope为前缀.key'的到值
  -->
  
  ${requestScope.key }	<br/>
  ${sessionScope.key }	<br/>
  ${applicationScope.key }<br/>
  
  ${phone } <br/>
  
  <!-- 默认或等于com.domain.Phone中的getBrand方法 -->
  ${phone.brand } <br/> 
  
  ${phone.price } <br/>
  ${phone.color } <br/>
  ${phone.size } <br/>
  
  ${param.tid }  <br/><!-- ${param["tid"] } -->
  
  <!-- 获取头部信息 -->
  ${header.host }<br />
  ${header.accept } <br />
  
  <!-- 输出随机数到页面 -->
  ${Math.random()} <br />
  
  <!-- 通过自定义标签获取字符串长度 -->
  ${Function.getLen(abc) } <br />
  
  <h3>接收List集合中的值</h3>
  
  ${list } <br />
  ${list[0] } 	  <!-- 获取集合中的第一个对象 --> <br />
  ${list[0].brand } <!-- 获取集合中的第一个对象的第一个值 --> <br />
  
  <!-- 遍历集合 -->
  <c:forEach items="${list }" var="p" >
      ${p.brand } ${p.price } ${p.color } ${p.size }
  </c:forEach>
  
  <%-- <%
      for(phone phone:list){
          out.println( phone.brand);
      }
  %> --%>
  
  <h3>接收map集合的值</h3>
  ${map } <br/>
  ${map.p1 } <br/>
  ${map.p1.brand } <br/>
  
  <h3>接收数组中的值</h3>
  ${Arrays.toString(arr) } <br/>	<!-- 需要导入java.util.arrays包 -->
  
  <c:forEach items="${arr }" var="i">
      ${i } <br />
  </c:forEach>
  
  <!-- 单选按钮 -->
  <input type="radio" name="sex" value="男" ${sex==0?'checked':''}> 男
  <input type="radio" name="sex" value="男" ${sex==1?'checked':''}> 女
  ```

#### 使用自己定义的具体方法

1. 新建一个类Function，在类中创建静态方法

2. 在**WEB-INF**下新建一个`tld`文件，在这个文件中设置自定义方法

   ```xm
   <?xml version="1.0" encoding="UTF-8"?>
   <taglib xmlns="http://java.sun.com/xml/nx/j2ee"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://java.sun.com/xml/ns/j2se web-jsptaglibrary_2_0.xsd" 
   version="2.0" >
   
   <description>my function</description>
   <tlib-version>1.0</tlib-version>
   <short-name>fff</short-name>
   <uri>/function.tld</uri>
   
       <function>
           <name>getLen</name> <!-- 方法名 -->
           <function-class>com.domain.Function</function-class> <!-- 方法所在的类 -->
           <function-signature>long getLen( java.lang.String )</function-signature> <!-- 定义函数实现的方法 -->
       </function>
   </taglib> 
   ```

3. 在JSP页面中通过`<%@ taglib %>`指令引入`tld`文件

   ```jsp
   <%@ taglib prefix="fff" uri="/function.tld" %>
   ```

4. 在EL表达式中就可以使用自定义方法

   ```jsp
   ${fff:getLen("abcdrf") }
   ${fff:reverse("abcdef")}
   ```

   