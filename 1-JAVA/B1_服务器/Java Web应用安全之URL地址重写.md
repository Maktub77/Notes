#### Java Web应用安全之URL地址重写

**一、背景**

公司的门户资讯网站的需求：对所有的静态页面做301 重定向（seo 提出的），例如：输入abc.com 能够重定向到 www.abc.com，输入abc.com/news 能够重定向到www.abc.com/news。

**二、301 重定向简介**

1. 首先要明白什么是重定向？

提到重定向我第一时间想到的是转发，因为这两者经常被拿到一起来做比较，也是我们在面试过程中经常被提到的一个问题。那么什么是重定向和转发？他们的区别又是什么？

简单来说，他们都是servlet 的两种请求方式

**转发：直接转发方式（Forward）**，客户端和浏览器只发出一次请求，Servlet、HTML、JSP或其它信息资源，由第二个信息资源响应该请求，在请求对象request中，保存的对象对于每个信息资源是共享的。

**重定向：间接转发方式（Redirect）**实际是两次HTTP请求，服务器端在响应第一次请求的时候，让浏览器再向另外一个URL发出请求，从而达到转发的目的。

他们的区别：

Forward 和Redirect 代表了两种请求转发方式：直接转发和间接转发。对应到代码里，分别是RequestDispatcher 类的forward() 方法和HttpServletRequest 类的sendRedirect() 方法。

对于直接方式，客户端浏览器只发出一次请求，Servlet把请求转发给Servlet、HTML、JSP 或其它信息资源，由第2个信息资源响应该请求，两个信息资源共享同一个request 对象。

对于间接方式，服务器端在响应第一次请求的时候，让浏览器再向另外一个URL 发出请求，从而达到转发的目的。它本质上是两次HTTP 请求，对应两个request 对象。

（详细介绍可以查看[JAVA常见面试题之Forward和Redirect的区别](https://www.cnblogs.com/selene/p/4518246.html)）

2. 301 重定向即页面永久性移走（301重定向）是一种非常重要的“自动转向”技术。网址重定向最为可行的一种办法。当用户或搜索引擎向网站服务器发出浏览请求时，服务器返回的HTTP数据流中头信息(header)中的状态码的一种，表示本网页永久性转移到另一个地址。还有一个是302 重定向，也叫暂时性转移(Temporarily Moved )，这里就不展开讲述。

**三、301 重定向使用场景**

1. 网站更换域名时，通过301永久重定向将旧域名重定向至新域名，挽回流量损失和SEO。
2. 当出于需要删除网站中的某些目录时，比如我要删除我博客下的博客导航，这时就可以用301永久重定向到网站首页。
3. 如果你有多个闲置域名时需要指向同一网站时，通过301永久重定向可以实现。
4. 你打算实现网址规范化（就是我们现在的需求）。

**四、301 重定向的实现**

1. 通过[java通过Filter过滤器实现html静态文件301重定向](http://www.zengdongwu.com/article14.html)，该方法需要自己写逻辑判断，拼接出需要重定向的url 值，再做301 重定向。
2. 伪静态urlrewrite，即使用Url Rewrite 进行URL 重写来实现网站的伪静态。

这里我们详细解释一下方法2，伪静态urlrewrite，它主要用到的是一个urlRewriteFilter 用于改写URL的Web过滤器。类似于Apache的mod_rewrite。适用于任何Web应用服务器（如Tomcat,jboss,jetty,Resin，Orion等）。其典型应用就把动态URL静态化，便于搜索引擎爬虫抓取你的动态网页。

第一步：添加`urlrewritefilter-4.0.4.jar`到`WEB-INF/lib`或者添加maven依赖： 

```xml
<dependency>
    <groupId>org.tuckey</groupId>
    <artifactId>urlrewritefilter</artifactId>
    <version>4.0.4</version>
</dependency>
```

第二步：编辑WEB-INF/web.xml 在其它servlet mapping前加入

```xml
<!-- html 301 请求监听 -->
<filter>
    <filter-name>UrlRewriteFilter</filter-name>
    <filter-class>org.tuckey.web.filters.urlrewrite.UrlRewriteFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>UrlRewriteFilter</filter-name>
    <url-pattern>/*</url-pattern>
    <dispatcher>REQUEST</dispatcher>
    <dispatcher>FORWARD</dispatcher>
</filter-mapping>
```

第三步：配置urlrewrite.xml 文件，添加跳转规则

```xml
<urlrewrite>
    <rule>
        <name>seo redirect 301</name>
        <condition name="host" operator="equal">^abc.com$</condition>
        <from>^/(.*)</from>
        <to type="permanent-redirect" last="true">http://www.abc.com/$1</to>;
    </rule>
</urlrewrite>　
```

标签说明：rule：url 重写规则，匹配规则有两种：regex （默认）和 wildcard

​                  name： 规则名称

​                  conditions：所有的conditions在rule拦截时都会执行（除非next设置为or）

​                  from：拦截请求的url 

​                  to：需求重定向的url

详细的标签说明可以查阅官网文档 <http://tuckey.org/urlrewrite/manual/3.0/index.html#install>

（另外说一下，项目之前有要求说增加多域名访问，最开始的做法是在 server.xml 文件中多增加一个<Alias>abc.com</Alias>。但是做了301 重定向的实现 之后发现这个可以去掉。）

第四步：重新启动应用 可以访问：<http://127.0.0.1:8080/rewrite-status> (或您本地Web应用程序和上下文的任何地址)查看重定向规则输出。 