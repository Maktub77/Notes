```java
package com.servlet;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import com.DTO.User;
import com.service.UserService;

@WebServlet("/LoginServlet")
public class LoginServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        request.setCharacterEncoding("utf-8");

        //得到用户在页面中输入的实时数据
        String userAccount = request.getParameter("userAccount");
        String psw = request.getParameter("psw");
        int adminFlag = Integer.parseInt(request.getParameter("adminFlag"));
        String checkCode = request.getParameter("checkCode");

        //把从CheckCodeServlet中放入session中的验证码获取得到
        HttpSession session = request.getSession();
        //		session.setMaxInactiveInterval(arg0); //设置session的有效时间以秒为单位，一般默认30分钟
        String autoCode = (String)session.getAttribute("autoCode");

        //调用service中的登录方法
        UserService us = new UserService();
        User user = us.login(new User(0, userAccount, null, psw, adminFlag)); 

        if(user != null){
            //判断验证码是否正确
            if(autoCode.equalsIgnoreCase(checkCode)){
                //主页面，并带过去账号
                session.setAttribute("user", user);
                request.getRequestDispatcher("index.jsp").forward(request, response);
            }else{
                //回到登录页面 ，并带过去错误信息
                request.setAttribute("errKey", "<div style='color:red'>验证码错误</div>");
                request.getRequestDispatcher("/login.jsp").forward(request, response);
            }
        }else{
            //回到登录页面 ，并带过去错误信息
            request.setAttribute("errKey", "<div style='color:red'>用户名、密码或身份错误</div>");
            request.getRequestDispatcher("/login.jsp").forward(request, response);
        }
    }
}

```

