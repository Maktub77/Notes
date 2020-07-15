### JSTL

- JavaServerPages Standard Tag Library   JSP标准标签库 

#### 使用步骤

1. 导入jar包-->WEB-INF/lib

2. 在JSP中引入所使用的标签库（c标签库,fmt标签库）

   ```jsp
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   ```

3. 使用(c标签库、fmt标签)

#### c标签

* out标签：输出值

  ```jsp
  <c:out value="abc"></c:out>
  
  <c:set var="v1" value="admin" scope="session"></c:set>
  <c:out value="${sessionScope.v1}" default="默认值"></c:out>
  
  <!-- escapexml默认或值为tru时，直接输出字符串到页面 -->
  <c:out value="<h1>hehe</h1>" escapeXml="false"></c:out>
  ```

* set标签

  * 用于设置各种web域(page,request,session,application)中的值

  ```jsp
  <c:set var="sess_key" value="sess_val" scope="session"></c:set>
  ${sess_key } <br />
  ```

  * 给对象赋值

  ```jsp
  <!-- 创建对象 -->
  <jsp:useBean id="u" class="com.domain.UserDTO"></jsp:useBean>
  
  <c:set target="${u }" property="username" value="王老吉"></c:set>
  <c:set target="${u }" property="password" value="123456"></c:set>
  ```

* remove标签：删除各种web域中的属性

  ```jsp
  <c:remove var="session_key"/>
  ```

* catch标签：捕获异常

  ```jsp
  <!-- 如果catch中的代码块有异常，则捕获后给变量myex -->
  <c:catch var="myex"> 
      <%
          String str=null;
          out.println(str.toString());
      %>
  </c:catch>
  
  <c:out value="${myex }"></c:out>
  ```

* if标签

  ```jsp
  <c:set var="x" value="20" scope="page"></c:set>
  <!--test得到的值 赋给 f1 并存放在 session 中时 -->
  <c:if test="${x>5 }" var="f1" scope="session">
      x的值是大于5的
  </c:if>
  <c:out value="${f1 }"></c:out>
  ```

* choose标签：类似于java中的if_else if_else

  ```jsp
  <c:set var="count" value="3"></c:set>
  <c:choose>
      <c:when test="${count==0 }">
          没有符合的条件-0!
      </c:when>
      <c:when test="${count==1 }">
          符合条件的有-1!
      </c:when>
      <c:when test="${count==2 }">
          没有符合的条件-2!
      </c:when>
      <c:otherwise>
          符合条件的有${count }
      </c:otherwise>
  </c:choose>
  ```

* foreach标签：循环

  ```jsp
  <%
  int [] arr ={1,2,3,4,5,6,7,8};
  pageContext.setAttribute("arr", arr);
  %>
  <!-- items代表数据源(需要遍历的集合或数组)，begin和end 是sub闭区间  从索引开始 -->
  <c:forEach items="${arr }" var="x" begin="0" end="7" step="2" varStatus="status">
      ${x } 索引是：${status.index }<br/>
  </c:forEach>
  
  <select name="age">
      <c:forEach begin="1" end="99" var="a">
          <option value="${a }">${a }</option>
      </c:forEach>
  </select>
  
  <!-- 表格隔行换色 -->
  <table width="400px" height="200px" border="1px" cellpadding="0" >
      <tr>
          <th>序号</th>
      </tr>
      <c:forEach begin="1" end="5" var="a" varStatus="sta">
          <c:if test="${sta.index mod 2 == 1}">
              <tr style="background-color:red">
                  <td>${a }</td>
              </tr>
          </c:if>
          <c:if test="${sta.index mod 2 == 0}">
              <tr style="background-color:green">
                  <td>${a }</td>
              </tr>
          </c:if>
      </c:forEach>
  </table>
  ```

* forTokens标签：遍历字符串

  ```jsp
  <c:forTokens items="spring,summer/autumn,winter" delims="/" var="token">
      ${token }<br/>
  </c:forTokens>
  ```

* URL路径标签 和 param标签

  ```jsp
  <!-- 使用绝对路径构造url -->
  <c:url value="http://localhost:8989/web06_JSTL/page1.jsp" var="myUrl1">
      <c:param name="name" value="张三"></c:param>
      <c:param name="country" value="中国"></c:param>
  </c:url>
  <a href="${myUrl1 }">page1</a>
  	相当于：
  <a href="http://localhost:8989/web06_JSTL/page1.jsp?name='李四'&country='中国'">page1</a>
  
  <hr/>
  <!-- 使用相对当前JSP页面的路径构造url -->
  <c:url value="page1.jsp?name=wangwu&country=France" var="myUrl2"></c:url>
  <a href="${myUrl2 }">page1</a>
  
  <hr/>
  <!-- 使用相对当前WEB应用的路径构造url -->
  <c:url value="/page1.jsp?name=zhaosi&country=France" var="myUrl3"></c:url>
  <a href="${myUrl3 }">page1</a>
  ```

* redirect标签：客户端跳转

  ```jsp
  <c:url value="page1.jsp" var="myUrl">
      <c:param name="name" value="张三三"></c:param>
      <c:param name="country" value="中国"></c:param>
  </c:url>
  <c:redirect url="${myUrl }"></c:redirect>
  ```

* import标签 : 导包

#### fmt标签

* 格式化标签

* 日期格式

  ```jsp
  <fmt:formatDate value="${now }"/><br />
  
  <fmt:formatDate value="${now }" pattern="yyyy年MM月dd日E HH:mm:ss"/><br />
  
  <fmt:formatDate value="${now }" type="time" timeStyle="short"/><br />
  
  <fmt:formatDate value="${now }" type="date" dateStyle="long"/><br />
  
  <fmt:formatDate value="${now }" type="both" dateStyle="short"/><br />
  ```

* 数字格式

  ```jsp
  <!-- 将数值转换为自定义格式 -->
  <fmt:formatNumber value="3.14159" pattern="￥0.00"></fmt:formatNumber><br />
  
  <fmt:formatNumber value="123456.123" pattern="#,#00.0#"></fmt:formatNumber><br />
  
  <!-- 将数值转换为百分比格式 -->
  <fmt:formatNumber value="0.2" type="percent"></fmt:formatNumber> <br />
  
  <!-- 将数值转换为货币格式 -->
  <fmt:formatNumber value="258563.1415" type="currency"></fmt:formatNumber><br />
  ```

* 获取货币名

  ```java
  String rmb = Currency.getInstance(Locale.CHINA).getCurrencyCode();
  String dollor = Currency.getInstance(Locale.US).getSymbol();
  
  session.setAttribute("rmb", rmb);
  session.setAttribute("dollor", dollor);
  ```

#### fn标签

* 内置函数

  ```jsp
  ${fn:length('abc') } <br/>
  
  ${fn:endsWith('c://hello.jpg','.txt') }<br/> <!-- false -->
  
  <c:forEach items="${fn:split('a,b,c,d',',') }" var="s">
      ${s } <br/>
  </c:forEach>
  ```

#### 自定义标签

1. 开发一个自定义标签处理类

   * 每个处理类都必须继承`javax.servlet.jsp.tagext.SimpleTagSupper`类
   * 如果标签有属性，则每个属性生成get、set方法
   * 重写doTag方法（标签要实现的功能）

2. 在WEB-INF下新建`**.tld`文件，每个tld文件对应一个标签库，每个标签库可以定义多个标签

   ```java
   <?xml version="1.0" encoding="UTF-8"?>
   
   <taglib xmlns="http://java.sun.com/xml/ns/j2ee"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-jsptaglibrary_2_0.xsd"
               version="2.0">
               
       <description>my Tag</description>
   	<tlib-version>1.0</tlib-version>
   	<short-name>mm</short-name>	
   	<uri>/date_tag.tld</uri>	<!-- 标签路径 -->	
   
   	<!-- 配置自定义标签 -->
       <tag>
   		<name>query</name>	<!-- 标签名 -->
   		<tag-class>com.mytag.QueryTag</tag-class>	<!-- 标签对应的类 -->
   		<body-content>empty</body-content>	<!-- 标签内为空 -->
   		<attribute>
   			<name>sql</name>	<!-- 标签属性名 -->
   			<required>true</required>
   			<fragment>true</fragment>
   		</attribute>
   	</tag>
   </taglib>
   ```

3. 在JSP中应用

   ```jsp
   <%@ taglib prefix="mm" uri="/date_tag.tld" %>    
   ```

   