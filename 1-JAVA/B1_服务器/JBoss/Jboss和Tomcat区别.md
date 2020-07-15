1. JBoss 是J2EE应用服务器，而Tomcat只是一个Servlet容器，或者说是一个简单的J2EE应用服务器

   JBoss中的Serclet容器还是tomcat

   与tomcat类似的Servlet容器有：Jetty(开源)，Resin(开源)……

   与JBoss类似的J2EE应用服务器有：Glassfish(开源)，Geronimo(开源)，WebLogic(商业)，WebSphere(商业)

2. tomcat是JSP/Servlet 容器

   JBoss是J2EE容器，J2EE包括JSP/Servlet，JMS，EJB，JAX-RS，CDI等等

   tomcat是完全开源，开源社区维护产品更新

   jboss有开源和企业化两个版本，企业化被Red Hat支持，一般支持10年，产品后继有保障

3. 注意JBoss和tomcat是不一样的，JBoss是一个可伸缩的服务器平台，当你的EJB程序编制完成后，如果访问量增加，只要通过服务器硬件就可以实现多台服务器同时运算，提高了负载容量，这个性能容量理论上是没有限制的，理论上无最大支持在线人数的上限，对于JBoss/EJB这样的平台来说，无最大访问量限制一说

   这是JBoss/EJB不同于Spring/Tomcat等平台的最大优点所在，而且EJB3.0也将出现轻量级解决方案，其实随着发展，已经模糊了轻量/重量的区别，如果还是以轻量/重量作为架构选择的标准，无疑是不明智的

   可伸缩性应该是架构选择的主要标准，所谓的可伸缩性，只在小型系统、一台服务器的情况下，我的系统也可以良好运转，多态服务器扩展后，我的系统只需通过增加硬件就可以实现性能扩展，无需修改太多软件