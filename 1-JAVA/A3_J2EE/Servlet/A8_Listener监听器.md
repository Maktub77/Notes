#### 在线监听

```java
package com.listener;

import java.util.HashMap;
import java.util.Map;

import javax.servlet.ServletContext;
import javax.servlet.http.HttpSession;
import javax.servlet.http.HttpSessionAttributeListener;
import javax.servlet.http.HttpSessionBindingEvent;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;

import org.apache.log4j.Logger;

public class OnlineListener implements HttpSessionListener, HttpSessionAttributeListener {

    private Logger log = Logger.getLogger(OnlineListener.class);

    public void sessionCreated(HttpSessionEvent se)  { 
        log.info("有人上线了！");
        //1.获取session对象
        HttpSession session = se.getSession();
        String sessionId = session.getId();//每一个会都有一个不一样的id

        //2.根据session获取application
        ServletContext application = session.getServletContext();

        //3.判断是否是一个新的会话
        if(session.isNew()){
            //获取刚刚登陆成功的用户信息
            String uname = (String)session.getAttribute("uname");
            if(uname==null){
                uname="游客";
            }
            //4.创建一个Map集合，用于存储登陆成功的用户
            Map<String, String> online = (Map<String, String>)application.getAttribute("online");
            if(online==null){
                online = new HashMap<>();
            }
            //将在线的用户名存入map集合中
            online.put(sessionId, uname);

            application.setAttribute("online", online);
        }

    }

    public void sessionDestroyed(HttpSessionEvent se)  { 
        log.info("有人下线了");
    }

    @Override
    public void attributeAdded(HttpSessionBindingEvent se) {
        //1.获取session对象
        HttpSession session = se.getSession();
        String sessionId = session.getId();//每一个会都有一个不一样的id

        //2.根据session获取application
        ServletContext application = session.getServletContext();

        //3.判断是否是一个新的会话
        String uname = (String)se.getValue();

        //4.创建一个Map集合，用于存储登陆成功信息
        Map<String, String> map = (Map<String, String>)application.getAttribute("online");
        if(map!=null){
            map.put(sessionId, uname);
        }
        application.setAttribute("online", map);
    }

    @Override
    public void attributeRemoved(HttpSessionBindingEvent se) {
        ServletContext application = se.getSession().getServletContext();
        Map<String, String> map = (Map<String, String>)application.getAttribute("online");
        if(map!=null){
            map.remove(se.getSession().getId());
        }

        application.setAttribute("online", map);
    }

    @Override
    public void attributeReplaced(HttpSessionBindingEvent se) {

    }

}
```

```java
String uname = request.getParameter("uname");

HttpSession session = request.getSession();

if(uname != null){
    session.setAttribute("uname", uname);
    request.getRequestDispatcher("index.jsp").forward(request, response);
}else{

}
```

```jsp
<h2>在线人员列表</h2>
<c:forEach items="${applicationScope.online }" var="p">
    ${p.key } --- ${p.value } <br/>
</c:forEach>
```

#### 安全退出

```java
HttpSession session = request.getSession();
session.removeAttribute("uname");
session.invalidate();  //session失效
response.sendRedirect("logout_result.jsp");
```

