配置阶段

* web.xml   	  dispatchServlet由Spring提供

* init-param       classpath:application.xml   contextConfigLocation
* url-patternn    /*

初始化，启动阶段

* init 			   加载Spring 配置文件
* IoC初始化         声明IoC容器，Map<String ,Object>
* scan-package   配置一个包路径，扫描到相关的类
* 实例化                将扫描到的相关类，利用反射机制实例化，并且保存到IoC容器中
* 依赖注入（DI）  自动给IoC容器中的对象，需要自动赋值的属性赋值

----

* HandlerMapping 将一个url对应一个Method,经这样的关系保存起来，暂时理解成是一个Map<String,Method>

等待运行，请求阶段

* doGet/doPost     request、response
* 从HandlerMapping 通过Request获取url，然后再去HandlerMapping中取值（无法匹配返回404）
* invoker    用反射调用Method
* response.write()



