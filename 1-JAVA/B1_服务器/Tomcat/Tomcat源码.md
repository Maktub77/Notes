Tomcat容器 Web服务器 servlet容器 tomcat是客户端请求提供服务的

```java
class Tomcat{

	//1.在一台服务器中监听一个端口，等待客户端的连接	
    //IO java网络编程 socket编程
    ServerSocket server = new ServerSocket(8000); //表示可以监听一个8000端口
    Socket socket = server.accept();
    //2.web服务器要为客户端的请求提供对应的资源 url http://
    //如何去获取并且返回给浏览器
    InputStream in = socket.getInputStream(); //浏览器的请求数据
    OutputStream out = socket.getOutputStream(); //用于向浏览器写数据
}
```

BootStrap.main

:arrow_down:

daemon.load(args);

:arrow_down:

Catalina.load()  	:arrow_right:	 conf/server.xml

:arrow_down:

load()中getServer().init()	:arrow_right:	声明init() Lifecycle ---- LifecycleBase.init();  	---- 根据你需要初始化的类去找，比如 StandardServer.initInternal/StandardService.initLnternal

:arrow_down:

//Initialize our defined Services

for(int i = 0;i < services.length; i++){

​	service[i].init();

}

:arrow_down:

connector.init();  	 :arrow_right: 		 (一定要进行ServerSocket的创建，不然和外部无法打交道)

:arrow_down:

protocolHandler.init()

:arrow_down:

AbstractProtocol.init()

:arrow_down:

endpoint.init();

:arrow_down:

bind();

:arrow_down:

​	NIO BIO

:arrow_down:

JIoEndpoint.bind()       :arrow_right:        ServerSockeFactory.createServerSocket()





