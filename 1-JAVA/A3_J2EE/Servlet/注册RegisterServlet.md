```java
package com.servlet;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.DTO.User;
import com.service.UserService;

@WebServlet("/RegisterServlet")
public class RegisterServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.处理乱码
        request.setCharacterEncoding("utf-8");

        //2.接受页面的数据
        String userAccount = request.getParameter("userAccount");
        String psw = request.getParameter("psw");
        String nickName = request.getParameter("nickName");
        String isAdmin = request.getParameter("adminFlag");
        int adminFlag = 0;
        if(isAdmin != null){
            adminFlag = Integer.parseInt(isAdmin);
        }

        //3.调用Service层中的方法(注册方法)
        UserService us = new UserService();
        boolean flag = us.register(new User(0,userAccount,nickName,psw,adminFlag));

        //4.根据前两步碰撞后的结果，跳转到不同的页面，响应给客户端
        if(flag){
            //页面跳转到登录界面
            request.setAttribute("userAccount", userAccount);
            request.getRequestDispatcher("login.jsp").forward(request, response);
        }else{
            //页面跳转到注册页面(回去)
            request.setAttribute("errKey", "<div style='color:red'>用户名已存在</div>");
            request.getRequestDispatcher("/register.jsp").forward(request, response);
        }
    }
}
```

