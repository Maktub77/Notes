### MVC

* M(model：模型层)
  * DTO：数据处理对象
  * DAO：数据访问接口
  * service：服务
* V(view：视图层)
  * JSP
    1. 收集客户端输入的数据
    2. 把服务器传递的数据进行展示
* C(controller：控制层)
  * Servlet
    * 从页面接收过来的值
    * 调用 模型层中的方法 进行操作
    * 根据第2步结果 -- 跳转页面

![](C:\Users\Maktub\Documents\Notes\0-picture\A0_Photo\MVC.png)