### Spring Boot 使用Swagger2构建RESTful API

* 构建RESTful API的目的通常都是由于多终端的原因，这些终端会共用很多底层业务逻辑，因此我们会抽象出这样一层来同时服务于多个移动端或者Web前端。

  * 问题：RESTful API就有可能面对多个开发人员或多个开发团队：IOS开发、Android开发或是Web开发等。

* 为了减少与其他团队开发期间频繁沟通成本

  * 传统做法：创建一份RESTful API文档来记录所有接口细节

    缺点：

    * 由于接口众多，并且细节复杂（需要考虑不同的HTTP请求类型、HTTP头部信息、HTTP请求内容等），高质量地创建这份文档本身就是件非常吃力的事，下游的抱怨声不绝于耳。
    * 随着时间推移，不断修改接口实现的时候都必须同步修改接口文档，而文档与代码又处于两个不同的媒介，除非有严格的管理机制，不然很容易导致不一致现象。

  * Swagger2

    * 可以轻松的整合到Spring Boot中，并与Spring MVC程序配合组织出强大RESTful API文档。
    * 减少我们创建文档的工作量，同时说明内容又整合入实现代码中，让维护文档和修改代码整合为一体，可以让我们在修改代码逻辑的同时方便的修改文档说明。
    * 提供了强大的页面测试功能来调试每个RESTful API。

 #### 添加Swagger2依赖

* pom.xml

  ```xml
  <dependency>
      <groupId>io.springfox</groupId>
      <artifactId>springfox-swagger2</artifactId>
      <version>2.2.2</version>
  </dependency>
  <dependency>
      <groupId>io.springfox</groupId>
      <artifactId>springfox-swagger-ui</artifactId>
      <version>2.2.2</version>
  </dependency>
  ```

#### 创建Swaggger2配置类

* 在`Application.java`同级创建Swagger2的配置类`Swagger2` 

  ```java
  @Configuration //让spring加载该类配置
  @EnableSwagger2	//启用Swagger2
  public class Swagger2 {
      @Bean
      //createRestApi函数创建Docket的Bean
      public Docket createRestApi() {
          return new Docket(DocumentationType.SWAGGER_2)
              .apiInfo(apiInfo()) //创建该Api的基本信息（这些基本信息会展现在文档页面中
              .select() //函数返回一个ApiSelectorBuilder实例用来控制哪些接口暴露给Swagger来展现
             //指定扫描的包路径来定义，Swagger会扫描该包下所有Controller定义的API，并产生文档内容（除了被@ApiIgnore指定的请求）
              .apis(RequestHandlerSelectors.basePackage("com.didispace.web"))
              .paths(PathSelectors.any())
              .build();
      }
  
      private ApiInfo apiInfo() {
          return new ApiInfoBuilder()
              .title("Spring Boot中使用Swagger2构建RESTful APIs")
              .description("更多Spring Boot相关文章请关注：http://blog.didispace.com/")
              .termsOfServiceUrl("http://blog.didispace.com/")
              .contact("程序猿DD")
              .version("1.0")
              .build();
      }
  }
  ```

#### 添加文档内容

* @ApiOperation：给API增加说明

* @ApiImplicitParams、@ApiImplicitParam：给参数增加说明

  ```java
  @ApiOperation(value="更新用户详细信息", notes="根据url的id来指定更新对象，并根据传过来的user信息来更新用户详细信息")
  @ApiImplicitParams({
      @ApiImplicitParam(name = "id", value = "用户ID", required = true, dataType = "Long"),
      @ApiImplicitParam(name = "user", value = "用户详细实体user", required = true, dataType = "User")
  })
  @RequestMapping(value="/{id}", method=RequestMethod.PUT)
  public String putUser(@PathVariable Long id, @RequestBody User user) {
      User u = users.get(id);
      u.setName(user.getName());
      u.setAge(user.getAge());
      users.put(id, u);
      return "success";
  }
  ```

#### 启动

* 启动Spring Boot程序，访问：<http://localhost:8080/swagger-ui.html> 

#### API文档访问与调试

* Swagger除了查看接口功能外，还提供了调试测试功能，我们可以点击上图中右侧的Model Schema（黄色区域：它指明了User的数据结构），此时Value中就有了user对象的模板，我们只需要稍适修改，点击下方`Try it out`按钮，即可完成了一次请求调用！ 
* 此时，你也可以通过几个GET请求来验证之前的POST请求是否正确。 





