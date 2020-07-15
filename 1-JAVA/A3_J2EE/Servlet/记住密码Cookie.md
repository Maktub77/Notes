* login.jsp

  ```jsp
  <script type="text/javascript">
      $(function(){
          //为id=uname的文本输入框，绑定一个按键松开的事件
          $("#uname").keyup(function(){
              var uname = $("#uname").val(); 
              $.ajax({ //ajax请求servlet
                  type:"POST",
                  url:"GetPassServlet",
                  data:"uname=" + uname,
                  success:function(msg){
                      $("#upass").val(msg);
                  }
              });	
          });
      });
  </script>
  
  记住密码：<input type="radio" name="remember" value="0" checked/>不记住<br/>
  <input type="radio" name="remember" value="1" />1天<br/>
  <input type="radio" name="remember" value="7" />1周<br/>
  ```

* LoginServlet

  ```java
  request.setCharacterEncoding("utf-8");
  
  String uname = request.getParameter("uname");
  String upass = request.getParameter("upass");
  int remember = Integer.parseInt(request.getParameter("remember"));
  
  //需要记住密码
  if(remember != 0){
      //需要记住密码
      //无论是否存储过都重新存储，客户端存储只能存储字符串类型
      Cookie cookie = new Cookie(uname, upass); 
  
      //设置存活时间
      cookie.setMaxAge(remember*60*60*24); //秒
      response.addCookie(cookie);//把值存储在客户端
      request.getRequestDispatcher("index.jsp").forward(request, response);
  }
  ```

* GetPassServlet

  ```java
  request.setCharacterEncoding("utf-8");
  String uname = request.getParameter("uname");
  //System.out.println(uname);
  Cookie [] cookie = request.getCookies();
  for(Cookie c :cookie){
  //  System.out.println(c.getName()+"----"+c.getValue());
      if(c.getName().equals(uname)){
          String upass = c.getValue();
          //System.out.println(upass);
          response.getOutputStream().write(upass.getBytes());
      }
  }
  ```

  