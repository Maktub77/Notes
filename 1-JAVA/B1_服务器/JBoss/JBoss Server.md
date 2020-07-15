jboss-5.1.0.GA

JDK1.6

操作系统XP

jboss-5.1.0.GA下载地址:http://jaist.dl.sourceforge.net/sourceforge/jboss/jboss-5.0.0.GA.zip

JDK1.6:  http://jaist.dl.sourceforge.net/sourceforge/jboss/jboss-5.0.0.GA-jdk6.zip

官方下载地址

JBoss 安装版下载地址：<http://labs.jboss.org/portal/jbossas/download/>

官方安装文档 <http://docs.jboss.com/jbossas/guides/installguide/r1/en/html_single/>

### 1. jboss-5.1.0.GA目录描述

##### JBoss **的目录结构说明**

| **目录**               | **描述**                                                     |
| ---------------------- | ------------------------------------------------------------ |
| bin                    | 启动和关闭 JBoss 的脚本（ run.bat 为 windows 系统下的启动脚本，shutdown.bat 为 windows 系统下的关闭脚本）。 |
| client                 | 客户端与 JBoss 通信所需的 Java 库（ JARs ）。                |
| docs                   | 配置的样本文件（数据库配置等）。                             |
| docs/dtd               | 在 JBoss 中使用的各种 XML 文件的 DTD 。                      |
| lib                    | 一些 JAR ， JBoss 启动时加载，且被所有 JBoss 配置共享。（不要把你的库放在这里） |
| server                 | 各种 JBoss 配置。每个配置必须放在不同的子目录。子目录的名字表示配置的名字。 JBoss 包含 3 个默认的配置： minimial ， default 和 all ，在你安装时可以进行选择。 |
| server/all             | JBoss 的完全配置，启动所有服务，包括集群和 IIOP 。           |
| server/default         | JBoss 的默认配置。在没有在 JBoss 命令行中指定配置名称时使用。 ( 我们下载的 JBOSS5.0 Beta4 版本默认采用此配置 )  。 |
| server/default/conf    | JBoss 的配置文件。                                           |
| server/default/data    | JBoss 的数据库文件。比如，嵌入的数据库，或者 JBossMQ         |
| server/default /deploy | JBoss 的热部署目录。放到这里的任何文件或目录会被 JBoss 自动部署。EJB 、 WAR 、 EAR ，甚至服务。 |
| server/default /lib    | 一些 JAR ， JBoss 在启动特定配置时加载他们。 (default 和 minimial 配置也包含这个和下面两个目录。 ) |
| server/default/log     | JBoss 的日志文件。                                           |
| server/default/tmp     | JBoss 的临时文件。                                           |

```
 
```

###  2.启动停止JBoss

安装好JBoss后，要确认安装是否成功，进入bin目录，执行run.bat或run.sh脚本。输出应该类似如下：

如果输出类似如上图，现在应该可以使用JBoss了。

访问:http://localhost:8080  (默认8080,自己可以改) 

### 3.JBOSS TOOLS

MyEclipse + jboss ,如果用 JBoss Tools 这个工具,比较好用.

官方网站: http://www.jboss.org/tools/download

现在的版本是 :JBoss Tools 3.1 

安装插件URL:  http://download.jboss.org/jbosstools/updates/development

安装插件的方式跟其它插件是一样的,这里就不说了.

这里以:MyEclipse Enterprise Workbench Version: 6.0.0 GA 为实例,发的截图:
 JBoss Tools 是一套基于Eclipse和JBoss 技术的开源项目，JBoss Tools的目的是为基于JBoss技术的项目开发提供一套完整工具支持，所有JBoss Plugins将作为JBDS（JBoss Developer Studio）的一部分，现在的JBoss Tools的所有的plugins 每天都会有一个新build包括最新的修改。
JBoss Tools目前包括有一下模块：

1. RichFaces VE : 这个模块是有Exadel提供的， 一个位于明斯克的非常有经验的和实力的团队，主要功能是为HTML和JSF 提供一个可是化的编辑器，可以方便灵活的设置JSF Project的各种配置文件
2. Seam Tools ： 支持seam-gen 以及于RichFaces VE的集成和对于Seam相关代码的refacetoring和编译等
3. Hibernate Tools ： 提供了一些project wizard，可以方便构建Hibernate所需的各种配置文件， 同时支持mapping 文件annotation和JPA的逆向工程以及交互式的HQL/JPA-QL/Criteria的执行。
4. JBoss AS ： 基于WTP Server Adapter框架， 将Eclipse与JBoss AS 完美集成的工具， 可以在Eclipse下方便的启动，停止 JBoss AS以及调试
5. JBPM Tools： 用于编辑和部署JBPM工作流
6. Struts Tools： 一套为基于Struts技术 开发 提供支持的工具
7. FreeMarker Tools: FreeMaker Editor ， 基于FreeMaker语法的， 可以高亮显示
8. JBossTools Core: JBoss Tools的核心模块
9. JBossWS Tools: JBossWS 是JBoss提供的Web Service runtime，JBossWS Tools的功能就是使用户在Eclipse你能方便的创建基于JBossWS 的web service project，用户可以轻松的完成从代码生成到打包、部署以及web service的调用和测试，开发者之需要关注业务逻辑的开发即可
10. Portlet Tools: 支持portlet开发的工具 
11. BIRT Tools: 基于BIRT的为jboss J2EE Server提供报表功能的工具
12. JBoss ESB Tooling: 提供一些 工程构建向导和关键的jboss-esb.xml的编辑器，支持工程的打包和部署。

###  4. JBoss 的配置

###### 日志文件设置

* 若需要修改JBoss默认的log4j设置，可修改JBoss安装目录"server"default"conf下的jboss-log4j.xml文件，在该文件中可以看到，log4j的日志输出在JBoss安装目录"server"default"log下的server.log文件中。对于log4j的设置，读者可以在网上搜索更加详细的信息。

###### web 服务的端口号的修改

* 这点在前文中有所提及，即修改JBoss安装目录"server"default"deploy"jboss-web.deployer下的server.xml文件，内容如下：

```xml
<Connector port="8080" address="${jboss.bind.address}"   

           maxThreads="250" maxHttpHeaderSize="8192"

           emptySessionPath="true" protocol="HTTP/1.1"

           enableLookups="false" redirectPort="8443" acceptCount="100"
           
           connectionTimeout="20000" disableUploadTimeout="true" />
```

将上面的8080端口修改为你想要的端口即可。重新启动JBoss后访问：<http://localhost/>:新设置的端口，可看到JBoss的欢迎界面。

###### JBoss 的安全设置

*  jmx-console 登录的用户名和密码设置
  * 默认情况访问 <http://localhost:8080/jmx-console> 就可以浏览jboss的部署管理的一些信息，不需要输入用户名和密码，使用起来有点安全隐患。下面我们针对此问题对jboss进行配置，使得访问jmx-console也必须要知道用户名和密码才可进去访问。步骤如下：

1. 找到JBoss安装目录/server/default/deploy/jmx-console.war/WEB-INF/jboss-web.xml文件，去掉<security-domain>java:/jaas/jmx-console</security-domain>的注释。修改后的该文件内容为：

 ```xml
<jboss-web>

    <!-- Uncomment the security-domain to enable security. You will

		need to edit the htmladaptor login configuration to setup the

		login modules used to authentication users.-->

    <security-domain>java:/jaas/jmx-console</security-domain>

</jboss-web>
 ```

2. 修改与i）中的jboss-web.xml同级目录下的web.xml文件，查找到<security-constraint/>节点，去掉它的注释，修改后该部分内容为：

```xml
<!-- A security constraint that restricts access to the HTML JMX console

   to users with the role JBossAdmin. Edit the roles to what you want and

   uncomment the WEB-INF/jboss-web.xml/security-domain element to enable

   secured access to the HTML JMX console.-->

<security-constraint>

    <web-resource-collection>

        <web-resource-name>HtmlAdaptor</web-resource-name>

        <description>An example security config that only allows users with the

            role JBossAdmin to access the HTML JMX console web application

        </description>

        <url-pattern>/*</url-pattern>

        <http-method>GET</http-method>

        <http-method>POST</http-method>

    </web-resource-collection>

    <auth-constraint>

        <role-name>JBossAdmin</role-name>

    </auth-constraint>

</security-constraint>
```

   在此处可以看出，为登录配置了角色JBossAdmin。

3. 在第一步中的jmx-console安全域和第二步中的运行角色JBossAdmin都是在login-config.xml中配置，我们在JBoss安装目录/server/default/config下找到它。查找名字为：jmx-console的application-policy：

 ```xml
<application-policy name = "jmx-console">
    <authentication>
        <login-module code="org.jboss.security.auth.spi.UsersRolesLoginModule"
                      flag = "required">
            <module-option name="usersProperties">props/jmx-console-users.properties</module-option>
            <module-option name="rolesProperties">props/jmx-console-roles.properties</module-option>
        </login-module>
    </authentication>
</application-policy>
 ```

在此处可以看出，登录的角色、用户等的信息分别在props目录下的jmx-console-roles.properties和jmx-console-users.properties文件中设置，分别打开这两个文件。

其中jmx-console-users.properties文件的内容如下：

```xml
# A sample users.properties file for use with the UsersRolesLoginModule

admin=admin
```

该文件定义的格式为：用户名=密码，在该文件中，默认定义了一个用户名为admin，密码也为admin的用户，读者可将其改成所需的用户名和密码。

jmx-console-roles.properties的内容如下：

 ```xml
# A sample roles.properties file for use with the UsersRolesLoginModule

admin=JBossAdmin, HttpInvoker
 ```

该文件定义的格式为：用户名=角色，多个角色以“,”隔开，该文件默认为admin用户定义了JBossAdmin和HttpInvoker这两个角色。

配置完成后读者可以通过访问： <http://localhost:8088/jmx-console/> ，输入jmx-console-roles.properties文件中定义的用户名和密码，访问jmx-console的页面。

###### web-console 登录的用户名和密码设置

默认情况下，用户访问JBoss的web-console时，不需要输入用户名和密码，为了安全起见，我们通过修改配置来为其加上用户名和密码。步骤如下：

1. 找到JBoss安装目录"server"default"deploy"management"console-mgr.sar"web-console.war"WEB-INF"jboss-web.xml文件，去掉<security-domain>java:/jaas/web-console</security-domain>的注释，修改后的文件内容为：

```xml
<?xml version='1.0' encoding='UTF-8' ?>

<!DOCTYPE jboss-web

   PUBLIC "-//JBoss//DTD Web Application 2.3V2//EN"

   "<http://www.jboss.org/j2ee/dtd/jboss-web_3_2.dtd>">

<jboss-web>

    <!-- Uncomment the security-domain to enable security. You will

   need to edit the htmladaptor login configuration to setup the

   login modules used to authentication users.-->

    <security-domain>java:/jaas/web-console</security-domain>

    <!-- The war depends on the -->

    <depends>jboss.admin:service=PluginManager</depends>

</jboss-web>
```

2. 打开1中jboss-web.xml同目录下的web.xml文件，去掉<security-constraint>部分的注释，修改后的该部分内容为：

```xml
<!-- A security constraint that restricts access to the HTML JMX console

   to users with the role JBossAdmin. Edit the roles to what you want and

   uncomment the WEB-INF/jboss-web.xml/security-domain element to enable

   secured access to the HTML JMX console.-->

<security-constraint>

    <web-resource-collection>

        <web-resource-name>HtmlAdaptor</web-resource-name>

        <description>An example security config that only allows users with the

            role JBossAdmin to access the HTML JMX console web application

        </description>

        <url-pattern>/*</url-pattern>

        <http-method>GET</http-method>

        <http-method>POST</http-method>

    </web-resource-collection>

    <auth-constraint>

        <role-name>JBossAdmin</role-name>

    </auth-constraint>

</security-constraint>
```

3. 打开JBoss安装目录"server"default"conf下的login-config.xml文件，搜索web-console，可找到如下内容：

```xml
<application-policy name = "web-console">

    <authentication>

        <login-module code="org.jboss.security.auth.spi.UsersRolesLoginModule"

                      flag = "required">

            <module-option name="usersProperties">web-console-users.properties</module-option>

            <module-option name="rolesProperties">web-console-roles.properties</module-option>

        </login-module>

    </authentication>

</application-policy>
```

  在文件中可以看到，设置登录web-console的用户名和角色等信息分别在login-config.xml文件所在目录下的web-console-users.properties和web-console-roles.properties文件中，但因为该目录下无这两个文件，我们在JBoss安装目录"server"default"conf"props目录下建立这两个文件，文件内容可参考在“jmx-console登录的用户名和密码设置”中的两个相应的配置文件的内容，web-console-users.properties文件的内容如下：

 ```xml
# A sample users.properties file for use with the UsersRolesLoginModule

admin=admin
 ```

web-console-roles.properties文件的内容如下：

```xml
# A sample roles.properties file for use with the UsersRolesLoginModule

admin=JBossAdmin,HttpInvoker
```

因为此时这两个文件不与login-config.xml同目录，所以login-config.xml文件需进行少许修改，修改后的<application-policy name = "web-console">元素的内容为：

```xml
<application-policy name = "web-console">

    <authentication>

        <login-module code="org.jboss.security.auth.spi.UsersRolesLoginModule"

                      flag = "required">

            <module-option name="usersProperties">props/web-console-users.properties</module-option>

            <module-option name="rolesProperties">props/web-console-roles.properties</module-option>

        </login-module>

    </authentication>

</application-policy>
```