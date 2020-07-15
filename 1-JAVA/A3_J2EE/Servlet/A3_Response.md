### Response

* 使用字节输出流，向客户端输出中文数据

* `ServletOutputStream sos = response.getOutputStream();`

  ```java
  response.setCharacterEncoding("utf-8");
  response.setContentType("text/html charset=utf-8");
  //使用字节输出流
  ServletOutputStream sos = response.getOutputStream();
  
  String msg = "Demo1Servlet:星期一";
  
  byte [] arr = msg.getBytes();//字符串转化为数组
  
  sos.write(arr);
  
  sos.flush();
  ```

* 使用字符输出流传输中文

* `PrintWriter pw = response.getWriter();`

  ```java
  //处理response响应给客户的编码方式
  response.setCharacterEncoding("utf-8");
  response.setContentType("text/html charset=utf-8");
  
  //response使用字符输出流，向客户端响应
  PrintWriter pw = response.getWriter();
  
  String msg = "Demon2Servlet:星期五";
  
  pw.println(msg);
  
  pw.flush();
  ```

#### 文件下载

* 页面

  ```jsp
  <h3>response：文件下载</h3>
  <a href="Demo3Servlet"><img src="imgs/风景.jpg" width="200px" height="135px"></a>
  ```

* Servlet

  ```java
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
      //得到资源在服务端项目中的位置
      //E:\J1805\apache-tomcat-8.0.35-windows-x86\apache-tomcat-8.0.35\wtpwebapps\web11_response\imgs\01.jpg(服务器路径)
      String imgPath = this.getServletContext().getRealPath("/imgs/风景.jpg");
  
      //获得资源名称
      String imgName = imgPath.substring(imgPath.lastIndexOf("\\")+1);
  
      //设置服务器传递给客户端的数据格式
      //response.setContentType("imges/jpg");
      //告诉客户端以附件形式获取响应
      response.setHeader("Content-disposition", "attachment;filename="+new String(imgName.getBytes("gb2312"),"ISO-8859-1"));
  
      //使用一个输入流指向服务器端的资源(01.jpg)
      InputStream is = new FileInputStream(imgPath);
  
      //从response对象中获取字节输出流，将获取到的内容，响应给客户端
      OutputStream os = response.getOutputStream();
  
      byte [] arr = new byte[1024];
      int len = 0; 
      while((len = is.read(arr)) != -1){
          os.write(arr, 0, len);
      }
      os.flush();
      os.close();
      os.close();
  }
  ```

