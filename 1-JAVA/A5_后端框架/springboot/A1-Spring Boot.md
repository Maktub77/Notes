## Spring boot

- 简化新Spring应用的初始搭建以及开发过程。

### 注解

#### @EnableAutoConfiguration

- 这个注解告诉Spring Boot根据你添加的jar依赖来“猜测”你想要如何配置Spring。自从spring-boot-starter-web添加了tomcat和SpringMVC之后，自动配置假定你正在开发一个Web应用程序并据此设置Spring

#### @SpringBootApplication

- Spring Boot 应用的标识
- Application很简单，一个main函数作为主入口。SpringApplication引导应用，并将Application本身作为参数传递给run方法。具体方法会启动嵌入式的Tomcat并初始化Spring环境及其各Spring组件。

### 部署

* Springboot 部署会采用两种方式：全部打包成一个jar，或者打包成一个war。 

#### jar方式

* `cd C:\Users\X7TI\Downloads\springboot mvn install` 这会导致在C:\Users\X7TI\Downloads\springboot 目录下生成一个jar文件 
* `java -jar target/springboot-0.0.1-SNAPSHOT.jar `
* 通过这种方式，把此jar上传到服务器并运行，就可达到部署的效果了 

#### war方式

* Application 修改

  * 新加@ServletCompomentScan注解，并且继承SpringBootServletInitializer 

    ```java
    @SpringBootApplication
    @ServletComponentScan
    public class Application extends SpringBootServletInitializer {
    
        @Override
        protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
            return application.sources(Application.class);
        }
    
        public static void main(String[] args) {
            SpringApplication.run(Application.class, args);
        }
    }
    ```

* pom.xml

  * 新加打包成war的声明： 

  ```xml
  <packaging>war</packaging>
  ```

  * spring-boot-starter-tomcat修改为 provided方式，以避免和独立 tomcat 容器的冲突.  表示provided 只在编译和测试的时候使用，打包的时候就没它了。 

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
      <scope>provided</scope>           
  </dependency>
  ```

  * 创建war包

    在 target 目录下 生成了一个 springboot-0.0.1-SNAPSHOT.war 文件 

    `cd C:\Users\X7TI\Downloads\springboot mvn clean package`

  * 重命名 war 包，然后部署 

    * 如果用 springboot-0.0.1-SNAPSHOT.war 这个文件名部署，那么访问的时候就要在路径上加上springboot-0.0.1-SNAPSHOT。 所以把这个文件重命名为 ROOT.war 然后把它放进tomcat 的webapps目录下。 

      > ROOT.war　并不是指访问的时候要使用 /ROOT/hello ,而是直接使用/hello 进行访问，ROOT表示根路径。 

  ### [JSP](http://how2j.cn/k/springboot/springboot-jsp/1647.html)

  ### 热部署

  1. 必须重启

     *  Springboot提供了热部署的方式，当发现任何类发生了改变，马上通过JVM类加载的方式，加载最新的类到虚拟机中。 这样就不需要重新启动也能看到修改后的效果了 

  2. pom.xml

     * 在pom.xml中新增加一个依赖和一个插件就行了 

       依赖：

     ```xml
     <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-devtools</artifactId>
         <optional>true</optional> <!-- 这个需要为 true 热部署才有效 -->
     </dependency>
     ```

     ​	插件：

     ```xml
     <plugin>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-maven-plugin</artifactId>
     </plugin>
     ```

### 异常统一处理使用场景

**@RestControllerAdvice** 是 @ControllerAdvice 和 @ResponseBody 的语义结合。是控制器增强，直接返回对象。这里用于统一拦截异常，然后返回错误码对象体。

- 在前后端开发中，经常用HTTP over JSON作为服务进行墙后段联调对接。

- 问题：很多涉及到返回码，错误码相关的处理；比如xxx参数不完整，权限不足，用户不存在等。

- 处理：Spring4.x提供的RestControllerAdvice,控制层通知器，这里用于同一拦截异常，进行响应式处理。

  ```java
  /**
   * 全局统一异常处理
   */
  @ControllerAdvice
  public class GlobalExceptionHandler {
      private static final Logger logger = LoggerFactory.getLogger(GlobalExceptionHandler.class);
      public static final String MESSAGE="message";
  
      /**
       * 自定义异常处理
       *
       * @param req
       * @param ex
       * @return
       * @throws Exception
       */
      @ExceptionHandler(value = RException.class)
      @ResponseBody
      public R baseExceptionHandler(HttpServletRequest req, RException ex) throws Exception {
          return R.error(ex);
      }
  
      /**
       * 404 异常处理
       *
       * @param req
       * @param ex
       * @return
       * @throws Exception
       */
      @ExceptionHandler(value = NoHandlerFoundException.class)
      @ResponseBody
      public R noHandlerExceptionHandler(HttpServletRequest req, Exception ex) throws Exception {
          return R.error(CodeDefined.URL_NOT_FOUND);
      }
  
      /**
       * 未知异常处理
       *
       * @param req
       * @param ex
       * @return
       * @throws Exception
       */
      @ExceptionHandler(value = Exception.class)
      @ResponseBody
      public R exceptionHandler(HttpServletRequest req, Exception ex) throws Exception {
  
          logger.error("未知异常:", ex);
          return R.error(CodeDefined.ERROR);
      }
  
      /**
       * 参数验证异常处理
       *
       * @param req
       * @param ex
       * @return
       * @throws Exception
       */
      @ExceptionHandler(value = {MethodArgumentNotValidException.class, BindException.class})
      @ResponseBody
      public R parameterExceptionHandler(HttpServletRequest req, Exception ex) {
          List<Map<String, String>> resErrors = new ArrayList<>();
          List<ObjectError> errors = null;
          if (ex instanceof MethodArgumentNotValidException) {
              errors = ((MethodArgumentNotValidException) ex).getBindingResult().getAllErrors();
          } else if (ex instanceof BindException) {
              errors = ((BindException) ex).getBindingResult().getAllErrors();
          }
  
          for (ObjectError error : errors) {
              Map<String, String> result = new HashMap<>();
              result.put(MESSAGE, "未知的参数错误");
  
              if (error instanceof FieldError) {
                  FieldError fieldError = (FieldError) error;
                  String messageFormat = fieldError.getDefaultMessage();
                  String errorMessage = formatValidMessage(messageFormat, fieldError);
  
                  result.put("field", fieldError.getField());
                  result.put(MESSAGE, errorMessage);
  
                  String value = String.valueOf(fieldError.getRejectedValue());
                  if (!StringUtils.isNulltoString(value)) {
                      result.put("value", value);
                  }
              } else {
                  result.put(MESSAGE, error.getDefaultMessage());
              }
              resErrors.add(result);
          }
  
  
          return R.error(CodeDefined.ERROR_PARAMETER).put("data", resErrors);
      }
  
      private String formatValidMessage(String format, FieldError error) {
  
          return format;
      }
  
      /**
       * 请求参数语法错误
       *
       * @param req
       * @param ex
       * @return
       * @throws Exception
       */
      @ExceptionHandler(value = HttpMessageNotReadableException.class)
      @ResponseBody
      public R httpMessageNotReadableExceptionHandler(HttpServletRequest req, Exception ex) throws Exception {
          logger.error("json语法错误异常:", ex);
          return R.error(CodeDefined.ERROR_SYNTAX);
      }
  
      /**
       * 密码验证 异常处理
       *
       * @param req
       * @param ex
       * @return
       * @throws Exception
       */
      @ExceptionHandler(value = IncorrectCredentialsException.class)
      @ResponseBody
      public R incorrectCredentialsExceptionHandler(HttpServletRequest req, Exception ex) throws Exception {
          return R.error(CodeDefined.ERROR_USER_OR_PASS);
      }
  
      /**
       * oauth2服务中token验证出错 异常处理
       *
       * @param req
       * @param ex
       * @return
       * @throws Exception
       */
      @ExceptionHandler(value = AuthenticationException.class)
      @ResponseBody
      public R authenticationExceptionHandler(HttpServletRequest req, Exception ex) throws Exception {
          return R.error(CodeDefined.ERROR_TOKEN);
      }
  
      /**
       * 请求方法错误
       *
       * @param req
       * @param ex
       * @return
       * @throws Exception
       */
      @ExceptionHandler(value = HttpRequestMethodNotSupportedException.class)
      @ResponseBody
      public R HttpRequestMethodNotSupportedException(HttpServletRequest req, HttpRequestMethodNotSupportedException ex) throws Exception {
          logger.error("请求方法类型错误:", ex);
          return R.error(CodeDefined.METHOD_ERROR.getValue(), String.format(
                  CodeDefined.METHOD_ERROR.getDesc(), ArrayUtil.splicing(",", ex.getSupportedMethods())));
      }
  }
  ```

---

### 配置切换

* 本地测试和上线使用使用不同的配置（端口...），此时就可以通过多配置文件实现多配制支持和灵活切换

* 多配制文件

  * 核心配置文件：application.properties

    ```pro
    spring.mvc.view.prefix=/WEB-INF/jsp/
    spring.mvc.view.suffix=.jsp
    spring.profiles.active=pro
    ```

  * 开发环境用的配置文件：application-dev.properties 

    ```properties
    server.port=8080
    server.context-path=/test
    ```

  * 生产环境用的配置文件：application-pro.properties 

    ```properties
    server.port=80
    server.context-path=/
    ```

* 部署

  * 不仅可以通过修改application.properties文件进行切换，还可以在部署环境下，指定不同的参数来确保生产环境总是使用的希望的那套配置。 

    ```
    cd C:\Users\X7TI\Downloads\springboot
    mvn install
    
    java -jar target/springboot-0.0.1-SNAPSHOT.jar --spring.profiles.active=pro
    或
    java -jar target/springboot-0.0.1-SNAPSHOT.jar --spring.profiles.active=dev
    ```

### Spring boot和ssm关系

1. 内置了tomcat 内部集成了tomcat ssm打包 war web工程、 boot 打包 是个jar
2. 配置简化

跟页面频繁交互 使用SSM+Web
