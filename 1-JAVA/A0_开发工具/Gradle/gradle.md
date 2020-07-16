# Gradle

* Gradle是一个基于Apache Ant和Apache Maven概念的项目自动化建构工具。
* 它使用一种基于Groovy的特定领域语言(DSL)来声明项目设置，抛弃了基于XML的各种繁琐配置。 面向Java应用为主。当前其支持的语言限于Java、Groovy和Scala，计划未来将支持更多的语言。 

---

**Groovy语言**

* 动态语言，和Java一样，运行在Java虚拟机中
* *Groovy是在 java平台上的、 具有像Python， Ruby 和 Smalltalk 语言特性的灵活动态语言， Groovy保证了这些特性像 Java语法一样被 Java开发者使用* 
* 可以看做Java语言的扩展

**Gradle**

* Gradle是一个工具，同时也是一个编程框架
* Gradle中，每一个待编译的工程都叫一个Project。每一个Project在构建的时候都包含一系列的Task。例如编译、打包这些构建过程的子过程都对应着一个Task。
* 一个Project到底包含多少个Task，其实是由编译脚本指定的plugin决定的。
  * **插件**就是用来**定义Task**，并具体**执行这些Task**的东西
* Gradle是一个框架，作为框架，它负责**定义流程和规则**。而具体的编译工作则是通过插件的方式来完成的。

**Gradle中每一个待编译的工程都是一个Project，一个具体的编译过程是由一个一个的Task来定义和执行的**。

---

* 每个Module下存在一个build.gradle文件，它定义了应用于本模块的构建规则
* 工程根目录下也存在一个build.gradle文件，它代表了整个工程的构建，其中定义了适用于这个工程中所有模块的构建规则
* Gradle配置文件
  * **gradle.properties**
    * 这个文件中定义了一系列“属性”。实际上，这个文件中定义了一系列供build.gradle使用的常量，比如keystore的存储路径、keyalias等等。 
  * **local.properties**
    * 这个文件中定义了一些本地属性，比如SDK的路径。 
  * **gradlew与gradlew.bat**
    * gradlew为Linux下的shell脚本，gradlew.bat是windows下的批处理文件
    * gradlew是gradle wrapper的缩写，也就是说他对gradle的命令进行了包装，比如我们进入到指定的Module目录并执行“gradlew.bat assemble”即可完成对当前Moudle的构建（windows系统下）
  * **setting.gradle**
    * 当一个项目中包含多个Module时，我们想要一次性构建所有的Module以完成整个项目的构建，使用这个文件。

----

### 常见配置

1. 依赖第三方库

   ```groovy
   dependencies {
       compile("org.springframework.boot:spring-boot-starter-aop")
   }
   ```

2. 导入本地jar包

   ```groovy
   dependencies { 
       compile files('lib/gson-2.8.0.jar')
   }
   ```

   * 加载某个目录下的jar包（以根目录lib文件夹为例，将所有jar包引入build.gradle配置 ）

   ```groovy
   dependencies { 
       compile fileTree(dir:'lib',include:['*.jar'])
   }
   ```

3. 依赖其他模块

   ```groovy
   dependencies {
       compile project(":cz-common")
   }
   ```

4. 配置属性

   * **buildscript**：声明Gradle脚本自身需要的资源。可以声明的资源包括依赖项、第三方插件、maven仓库地址等
   * **ext**：自定义属性
   * **repositories**：仓库
   * **dependencies**：把需要配置的依赖用classpath配置上，因为这个dependencies在buildscript{}里面，所以代表的是Gradle需要的插件

   ```groovy
   buildscript {
       ext {
           springBootVersion = '2.0.4.RELEASE'
       }
       repositories {
           maven { url 'http://maven.aliyun.com/nexus/content/groups/public/' }
       }
       dependencies {
           classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
       }
   }
   ```

   * **allprojects:**中定义的属性会被应用到所有 module 中，但是为了保证每个项目的独立性，我们一般不会在这里面操作太多共有的东西 

5. 部署web项目（假设你把所有静态文件放在static目录，所有的web组件放在webfiles ）

   ```groovy
   webAppDirName = 'webfiles'
   war {
   	from 'static'
   }
   
   ```

6. 设置打包的编码

   * build.gradle中设置

   ```groovy
   tasks.withType(JavaCompile) {  
       options.encoding = "UTF-8"  
   }
   ```

   * 或者找到gradle的安装目录，打开gradle/bin/gradle.bat 

     ```
     set DEFAULT_JVM_OPTS="Dfile.encoding=UTF-8"
     ```

   * 在Idea中

     配置Setting--->Gradle--->Gradle VM options：-Dfile.encoding=utf-8

7. 打包成war包

   ```groovy
   apply plugin: 'war' //打包成war包
   
   sourceCompatibility = 1.8   // 设置 JDK 版本
   webAppDirName = 'web'    // 设置 WebApp 根目录，默认是webapp目录
   // 设置 Java 源码所在目录
   sourceSets {
       main {
           java {
               srcDir 'src'
           }
           resources {
               srcDir 'resources'
           }
       }
   }
   
   // 设置 maven 库地址
   repositories { 
       mavenCentral()//使用maven的中央仓库去下载地址
    //   maven { url "http://maven.aliyun.com/nexus/content/groups/public/" }  
    //   jcenter()  
    //   google()
   }
   
   dependencies {
       //设置编码为UTF-8，默认GBK
       tasks.withType(JavaCompile) {
           options.encoding = "UTF-8"
       }
       //将本地文件夹中的所有jar包打进去
       compile fileTree(dir: 'web/WEB-INF/lib', include: ['*.jar'])
       // 没有网络的时候用这个
       compileOnly files('C:/java/tomcat/apache-tomcat-8.0.28/lib/servlet-api.jar')
   
       // 有网络是用maven
       // providedCompile 'javax.servlet:servlet-api:2.5' // 编译期
   }
   ```

> 在项目根目录打开命令行，执行：`gradle build` 会在build/libs下生成war包

---

### Groovy语法

#### 变量

* 在groovy中，没有固定的类型，变量通过def关键字引用

  ```groovy
  def name = 'sheldon'
  ```

* 我们通过单引号引用一串字符串的时候这个字符串只是单独的字符串，但是如果使用双引号引用，在字符串里面还支持**插值**操作

  ```groovy
  def name = 'Andy'
  def greeting = "Hello, $name!"
  ```

#### **方法**

类似python一样，通过def关键字定义一个方法。方法如果不指定返回值，默认返回最后一行代码

```groovy
def square(def num){
    num*num
}
square 4
```

#### 类

Groovy也是通过Groovy定义一个类

```groovy
class MyGroovyClass{
    String greeting
    String getGreeting{
        return 'Hello!'
    }
}
```

* 在Groovy中，默认所有的类和方法都是public的，所有类的字段都是private的；

* 和java一样，我们通过new关键字得到类的实例，使用def接受对象的引用

  ```groovy
  def instance = new MyGroovyClass（）
  ```

* 而且在类中声明的字段都默认会生成对应的setter，getter方法。所以上面的代码我们可以直接调用`instance.setGreeting 'Hello，Groovy！'`

  > 注：Groovy的方法调用是可以没有括号的，而且也不需要分号结尾

* 我们乐意直接通过`instance.greeting`这样的方式拿到字段值，但是这也会通过其get方法，而不是直接拿到这个值

#### map、collections

* 在Groovy中，定义一个列表

  ```groovy
  List list = [1,2,3,4,5]
  ```

* 遍历列表

  ```groovy
  list.each(){element ->
      println element
  }
  ```

* 定义一个map

  ```groovy
  Map pizzaPrices = [margherita:10, pepperoni:12]
  ```

* 获取一个map值

  ```
  pizzaPrices.get('pepperoni')
  pizzaPrices['pizzaPrices']
  ```

#### 闭包

* 在Groovy中有一个**闭包**的概念。闭包可以理解为就是Java中的匿名内部类。闭包支持类似lamda形式的语法调用。

  ```groovy
  def square = {num->
      num*num
  }
  square 8
  ```

  如果只有一个参数，我们甚至可以省略这个参数，默认使用it作为参数，最后代码

  ```groovy
  Closure square = {
      it * it
  }
  square 16
  ```

  理解闭包的语法后，我们会发现，其实在我们之前的配置文件里，dependencies这些后面紧跟的代码块，就是一个闭包而已。

