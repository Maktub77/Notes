# Maven

http://www.runoob.com/maven/maven-tutorial.html

* 基于项目对象模型（缩写：POM）概念，Maven利用一个中央信息片断能管理一个项目的构建、报告和文档等步骤。Maven 是一个项目管理工具，可以对 Java 项目进行构建、依赖管理。

## Maven POM

POM( Project Object Model，项目对象模型 ) 是 Maven 工程的基本工作单元，是一个XML文件，包含了项目的基本信息，用于描述项目如何构建，声明项目依赖，等等。

执行任务或目标时，Maven 会在当前目录中查找 POM。它读取 POM，获取所需的配置信息，然后执行目标。

POM 中可以指定以下配置：

- 项目依赖
- 插件
- 执行目标
- 项目构建 profile
- 项目版本
- 项目开发者列表
- 相关邮件列表信息

在创建 POM 之前，我们首先需要描述项目组 (groupId), 项目的唯一ID。

```xml
<project xmlns = "http://maven.apache.org/POM/4.0.0"
         xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation = "http://maven.apache.org/POM/4.0.0                          http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <!-- 模型版本 -->
    <modelVersion>4.0.0</modelVersion>
    <!-- 公司或者组织的唯一标志，并且配置时生成的路径也是由此生成， 如com.companyname.project-group，maven会将该项目打成的jar包放本地路径：/com/companyname/project-group -->
    <groupId>com.companyname.project-group</groupId>
    <!-- 项目的唯一ID，一个groupId下面可能多个项目，就是靠artifactId来区分的 -->
    <artifactId>project</artifactId>
    <!-- 版本号 -->
    <version>1.0</version>
</project>
```

所有的POM文件都需要project元素和三个必需字段：`groupId`,`artifactId`,`version`.

| 节点         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| project      | 工程的根标签。                                               |
| modelVersion | 模型版本需要设置为 4.0。                                     |
| groupId      | 这是工程组的标识。它在一个组织或者项目中通常是唯一的。例如，一个银行组织 com.companyname.project-group 拥有所有的和银行相关的项目。 |
| artifactId   | 这是工程的标识。它通常是工程的名称。例如，消费者银行。groupId 和 artifactId 一起定义了 artifact 在仓库中的位置。 |
| version      | 这是工程的版本号。在 artifact 的仓库中，它用来区分不同的版本。例如：<br>`com.company.bank:consumer-banking:1.0 ` `com.company.bank:consumer-banking:1.1` |

### 父（Super）POM

父（Super）POM是 Maven 默认的 POM。所有的 POM 都继承自一个父 POM（无论是否显式定义了这个父 POM）。父 POM 包含了一些可以被继承的默认设置。因此，当 Maven 发现需要下载 POM 中的 依赖时，它会到 Super POM 中配置的默认仓库 http://repo1.maven.org/maven2 去下载。

Maven 使用 effective pom（Super pom 加上工程自己的配置）来执行相关的目标，它帮助开发者在 pom.xml 中做尽可能少的配置，当然这些配置可以被重写

使用以下命令来查看 Super POM 默认配置：

```
mvn help:effective-pom
```

1. 接下来我们创建目录 MVN/project，在该目录下创建 pom.xml，内容如下：

```xml
<project xmlns = "http://maven.apache.org/POM/4.0.0"
    xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation = "http://maven.apache.org/POM/4.0.0
    http://maven.apache.org/xsd/maven-4.0.0.xsd">
 
    <!-- 模型版本 -->
    <modelVersion>4.0.0</modelVersion>
    <!-- 公司或者组织的唯一标志，并且配置时生成的路径也是由此生成， 如com.companyname.project-group，maven会将该项目打成的jar包放本地路径：/com/companyname/project-group -->
    <groupId>com.companyname.project-group</groupId>
 
    <!-- 项目的唯一ID，一个groupId下面可能多个项目，就是靠artifactId来区分的 -->
    <artifactId>project</artifactId>
 
    <!-- 版本号 -->
    <version>1.0</version>
</project>
```

2. 在命令控制台，进入 MVN/project 目录，执行以下命令：

```
C:\MVN\project>mvn help:effective-pom
```

3. Maven 将会开始处理并显示 effective-pom。

4. Effective POM 的结果就像在控制台中显示的一样，经过继承、插值之后，使配置生效。

   在上面的 pom.xml 中，你可以看到 Maven 在执行目标时需要用到的默认工程源码目录结构、输出目录、需要的插件、仓库和报表目录。

   Maven 的 pom.xml 文件也不需要手工编写。

   Maven 提供了大量的原型插件来创建工程，包括工程结构和 pom.xml。

##　Ｍaven构建生命周期

Maven 构建生命周期定义了一个项目构建跟发布的过程。

一个典型的 Maven 构建（build）生命周期是由以下几个阶段的序列组成的：

| 阶段             | 处理     | 描述                                                     |
| ---------------- | -------- | -------------------------------------------------------- |
| 1. 验证 validate | 验证项目 | 验证项目是否正确且所有必须信息是可用的                   |
| 2. 编译 compile  | 执行编译 | 源代码编译在此阶段完成                                   |
| 3. 测试 Test     | 测试     | 使用适当的单元测试框架（例如JUnit）运行测试。            |
| 4. 包装 package  | 打包     | 创建JAR/WAR包如在 pom.xml 中定义提及的包                 |
| 5. 检查 verify   | 检查     | 对集成测试的结果进行检查，以保证质量达标                 |
| 6. 安装 install  | 安装     | 安装打包的项目到本地仓库，以供其他项目使用               |
| 7. 部署 deploy   | 部署     | 拷贝最终的工程包到远程仓库中，以共享给其他开发人员和工程 |

为了完成 default 生命周期，这些阶段（包括其他未在上面罗列的生命周期阶段）将被按顺序地执行。

Maven 有以下三个标准的生命周期：

- **clean**：项目清理的处理

  - pre-clean：执行一些需要在clean之前完成的工作
  - clean：移除所有上一次构建生成的文件
  - post-clean：执行一些需要在clean之后立刻完成的工作

- **default(或 build)**：项目部署的处理

  | 生命周期阶段          | 描述                                                         |
  | --------------------- | ------------------------------------------------------------ |
  | validate              | 检查工程配置是否正确，完成构建过程的所有必要信息是否能够获取到。 |
  | initialize            | 初始化构建状态，例如设置属性。                               |
  | generate-sources      | 生成编译阶段需要包含的任何源码文件。                         |
  | process-sources       | 处理源代码，例如，过滤任何值（filter any value）。           |
  | generate-resources    | 生成工程包中需要包含的资源文件。                             |
  | process-resources     | 拷贝和处理资源文件到目的目录中，为打包阶段做准备。           |
  | compile               | 编译工程源码。                                               |
  | process-classes       | 处理编译生成的文件，例如 Java Class 字节码的加强和优化。     |
  | generate-test-sources | 生成编译阶段需要包含的任何测试源代码。                       |
  | process-test-sources  | 处理测试源代码，例如，过滤任何值（filter any values)。       |
  | test-compile          | 编译测试源代码到测试目的目录。                               |
  | process-test-classes  | 处理测试代码文件编译后生成的文件。                           |
  | test                  | 使用适当的单元测试框架（例如JUnit）运行测试。                |
  | prepare-package       | 在真正打包之前，为准备打包执行任何必要的操作。               |
  | package               | 获取编译后的代码，并按照可发布的格式进行打包，例如 JAR、WAR 或者 EAR 文件。 |
  | pre-integration-test  | 在集成测试执行之前，执行所需的操作。例如，设置所需的环境变量。 |
  | integration-test      | 处理和部署必须的工程包到集成测试能够运行的环境中。           |
  | post-integration-test | 在集成测试被执行后执行必要的操作。例如，清理环境。           |
  | verify                | 运行检查操作来验证工程包是有效的，并满足质量要求。           |
  | install               | 安装工程包到本地仓库中，该仓库可以作为本地其他工程的依赖。   |
  | deploy                | 拷贝最终的工程包到远程仓库中，以共享给其他开发人员和工程。   |

  当一个阶段通过 Maven 命令调用时，例如 mvn compile，只有该阶段之前以及包括该阶段在内的所有阶段会被执行。

  不同的 maven 目标将根据打包的类型（JAR / WAR / EAR），被绑定到不同的 Maven 生命周期阶段。

  ### 命令行调用

  在开发环境中，使用下面的命令去构建、安装工程到本地仓库

  ```
  mvn install
  ```

  这个命令在执行 install 阶段前，按顺序执行了 default 生命周期的阶段 （validate，compile，package，等等），我们只需要调用最后一个阶段，如这里是 install。

  在构建环境中，使用下面的调用来纯净地构建和部署项目到共享仓库中

  ```
  mvn clean deploy
  ```

  这行命令也可以用于多模块的情况下，即包含多个子项目的项目，Maven 会在每一个子项目执行 clean 命令，然后再执行 deploy 命令。

- **site**：项目站点文档创建的处理

  Maven Site 插件一般用来创建新的报告文档、部署站点等。

  - pre-site：执行一些需要在生成站点文档之前完成的工作
  - site：生成项目的站点文档
  - post-site： 执行一些需要在生成站点文档之后完成的工作，并且为部署做准备
  - site-deploy：将生成的站点文档部署到特定的服务器上

## Maven 阿里云(Aliyun)仓库

Maven 仓库默认在国外， 国内使用难免很慢，我们可以更换为阿里云的仓库。

第一步:修改 maven 根目录下的 conf 文件夹中的 setting.xml 文件，在 mirrors 节点上，添加内容如下：

```xml
<mirrors>
    <mirror>
      <id>alimaven</id>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      <mirrorOf>central</mirrorOf>        
    </mirror>
</mirrors>
```

![img](http://www.runoob.com/wp-content/uploads/2018/09/455FA277-3216-4BA5-94D3-0F650C763D6F.png)

第二步: pom.xml文件里添加：

```xml
<repositories>  
    <repository>  
        <id>alimaven</id>  
        <name>aliyun maven</name>  
        <url>http://maven.aliyun.com/nexus/content/groups/public/</url>  
        <releases>  
            <enabled>true</enabled>  
        </releases>  
        <snapshots>  
            <enabled>false</enabled>  
        </snapshots>  
    </repository>  
</repositories>
```

# Maven 引入外部依赖

**如果我们需要引入第三库文件到项目，该怎么操作呢？**

pom.xml 的 dependencies 列表列出了我们的项目需要构建的所有外部依赖项。

要添加依赖项，我们一般是先在 src 文件夹下添加 lib 文件夹，然后将你工程需要的 jar 文件复制到 lib 文件夹下。我们使用的是 ldapjdk.jar ，它是为 LDAP 操作的一个帮助库：

![img](http://www.runoob.com/wp-content/uploads/2018/09/11-external-project-structure.jpg)

然后添加以下依赖到 pom.xml 文件中：

```xml
<dependencies>
    <!-- 在这里添加你的依赖 -->
    <dependency>
        <groupId>ldapjdk</groupId>  <!-- 库名称，也可以自定义 -->
        <artifactId>ldapjdk</artifactId>    <!--库名称，也可以自定义-->
        <version>1.0</version> <!--版本号-->
        <scope>system</scope> <!--作用域-->
        <systemPath>${basedir}\src\lib\ldapjdk.jar</systemPath> <!--项目根目录下的lib文件夹下-->
    </dependency> 
</dependencies>
```

pom.xml 文件完整代码如下：

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
                             http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.companyname.bank</groupId>
    <artifactId>consumerBanking</artifactId>
    <packaging>jar</packaging>
    <version>1.0-SNAPSHOT</version>
    <name>consumerBanking</name>
    <url>http://maven.apache.org</url>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.1</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>ldapjdk</groupId>
            <artifactId>ldapjdk</artifactId>
            <scope>system</scope>
            <version>1.0</version>
            <systemPath>${basedir}\src\lib\ldapjdk.jar</systemPath>
        </dependency>
    </dependencies>
</project>
```

