### JSP(Java Server Pages)

* JSP全称Java Server  Pages，是一种动态网页开发技术。它使用JSP标签在HTML网页中插入Java代码。标签通常以<%开头以%>结束。
* JSP技术是以Java语言作为脚本语言的，JSP网页为整个服务器端的Java库单元提供了一个接口来服务于HTTP的应用程序。
* JSP开发的WEB应用可以跨平台使用，既可以运行在Linux上也能运行在Window上。

#### 编译指令

* Page指令

  * 为容器提供当前页面的使用说明。一个JSP页面可以包含多个page指令。

  * 属性

    * import
    * pageEncoding
    * contentType
    * isErrorPage

    ... ...

  ```jsp
  <%@ page attribute="value" %>
  ```

* Include指令(静态包含  页头、页尾包含进来)

  * JSP可以通过include指令来包含其他文件。被包含的文件可以是JSP文件、HTML文件或文本文件。包含的文件就好像是该JSP文件的一部分，会被同时编译执行。
  * Include指令中的文件名实际上是一个相对的URL。如果您没有给文件关联一个路径，JSP编译器默认在当前路径下寻找。

  ```jsp
  <%@ include file="relative url" %>
  ```

* Taglib指令

  * 引入一个自定义标签集合的定义，包括库路径、自定义标签。
  * uri属性确定标签库的位置，prefix属性指定标签库的前缀。

  ```jsp
  <%@ taglib uri="uri" prefix="prefixOfTag" >
  ```

> 静态include:在编译时两个文件就合并了
>
> 动态include:不会合并文件，当代码执行到include时，才包含另一个文件

#### 动作指令

* `<jsp:inculde>`

  * 动态包含--用来包含静态和动态的文件。该动作把指定文件插入正在生成的页面。

  * ```
    <jsp:include page="relative URL" flush="true" />
    ```

    以下是include动作相关的属性列表。

    | 属性  | 描述                                       |
    | ----- | ------------------------------------------ |
    | page  | 包含在页面中的相对URL地址。                |
    | flush | 布尔属性，定义在包含资源前是否刷新缓存区。 |

* `<jsp:useBean>`

  * 用来装载一个将在JSP页面中使用的JavaBean

  * ```
    <jsp:useBean id="name" class="package.class" />
    ```

    在类载入后，我们既可以通过 jsp:setProperty 和 jsp:getProperty 动作来修改和检索bean的属性。 

    以下是useBean动作相关的属性列表。

    | 属性     | 描述                                                        |
    | -------- | ----------------------------------------------------------- |
    | class    | 指定Bean的完整包名。                                        |
    | type     | 指定将引用该对象变量的类型。                                |
    | beanName | 通过 java.beans.Beans 的 instantiate() 方法指定Bean的名字。 |

* `<jsp:setProperty>`

  * 用来设置已经实例化的Bean对象的属性

* `<jsp:getProperty>`

  * 提取指定Bean属性的值，转换成字符串，然后输出。

* `<jsp:forward>`

  * 请求转发:地址不变，但是页面跳转到指定页面

* `<jsp:param>`

  ```jsp
  <!-- 静态include:在编译时连个文件就合并了
  动态include:不会合并文件，当代码执行到include时，才包含另一个文件 
  -->
  <jsp:include page="page2.jsp"></jsp:include>
  
  <!-- 请求转发:地址不变，但是页面跳转到指定页面 -->
  <%-- <jsp:forward page="page3.jsp">
  		<jsp:param value="whaha" name="uname" ></jsp:param>
  	</jsp:forward> --%>
  
  <jsp:useBean id="dog1" class="com.domain.Dog">
      <jsp:setProperty name="dog1" value="小狗" property="name" />
      <jsp:setProperty name="dog1" value="10" property="age" />
  </jsp:useBean>
  
  <%=dog1 %>
  
  <jsp:getProperty property="name" name="dog1"/>
  <jsp:getProperty property="age" name="dog1"/>
  ```

