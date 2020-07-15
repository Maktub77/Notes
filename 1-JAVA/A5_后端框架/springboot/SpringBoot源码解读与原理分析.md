@SpringBootApplication是组合注解

1. @ComponentScan默认扫描当前配置类所在包及子包下的所有组件，exclude属性会将主启动类、自动配置类屏蔽掉
2. @Configuration可标注配置类，@SpringBootConfiguration 并没有对其做实质性扩展

---

SpringFramework的手动装配

1. SpringFramework提供了模式注解、@EnableXXX+@Import的组合手动装配
2. @SpringBootApplication 标注的主启动类所在包会被视为扫描包的根包

---

1. AutoConfigurationImportSelector 配合 SpringFactoriesLoader 可加载 “META-INF/spring.factories”中配置的 @EnableAutoCOnfiguration 对应的自动装配类
2. DeferredImportSelector 的执行时机比ImportSelector 更晚
3. SpringFramework 实现了自己的SPI技术，相比较于Java原生的SPI更灵活

---

1. 自动装配类是有执行顺序的，WebMvcAutoConfigration的执行顺序在ServletWebServerFactoryAutoConfiguratuon、DispatcherSercletAutoConfiguration之后
2. SpringBoot会根据当前classpath下的类来决定装配哪些组件、启动那种类型的Web容器
3. WebMvc的配置包括消息转换器、视图解析器、处理器映射器、处理器适配器、静态资源映射配置、主页设置、应用图标设置等
4. 配置SpringBoot应用除了可以使用properties、yml之外，还可以使用Customizer来编程式配置

---

ApplicationContext的接口继承

```java
public interface ApplicationContext extends EnvironmentCapable, ListableBeanFactory, HierarchicalBeanFactory,
MessageSource, ApplicationEventPublisher, ResourcePatternResolver
```

BeanFactory

* ListableBeanFactory：可以提供bean的迭代
* HierarchicalBeanFactory：实现多层嵌套容器的支撑

EnvironmentCapable

```java
public interface EnvironmentCapable {

    /**
	 * Return the {@link Environment} associated with this component.
	 */
    Environment getEnvironment();

}
```

MessageSource：实现国际化的接口

ApplicationEvenPublisher：封装发布功能的接口

ResourcePatternResolver：资源模式解析器 

---

ApplicationContext——>ConfigurableApplicationContext——>AbstractApplicationContext——>AbstractRefreshableApplicationContext——>AbstractRefrashableConfigApplicationContext——>AbstractXmlApplicationContext——>ClassPathXmlApplicationContext

ClassPathXmlApplicationContext：基于xml，可刷新的，可配置的

---

Springboot对IOC容器的扩展

* WebServerApplicationContext

  ```java
  public interface WebServerApplicationContext extends ApplicationContext
  ```

  由创建和管理嵌入式Web服务器的生命周期的应用程序上下文实现的接口

* ConfigurableWebServerApplication

---

1. SpringFramework原生的IOC容器的特点：分层次的、可列举的、可配置的
2. SpringBoot在SpringFramework原生的IOC容器上做了扩展，且都是基于注解的扩展

---

IOC：SpringBoot准备IOC容器

1. main方法进入

   ```java
   @SpringBootApplication
   public class DemoApplication{
       public static void main（String[] args）{
           SpringApplication.run(DemoApplication.calss. args);
       }
   }
   ```

2. 进入SpringApplication.run方法

3. new SpringApplication(primarySources)：创建SpringApplication

   ```java
   private Set<Class<?>> primarySources;
   
   public SpringApplication(Class<?>... primarySources) {
       this(null, primarySources);
   }
   
   @SuppressWarnings({ "unchecked", "rawtypes" })
   public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
       // resourceLoader为null
       this.resourceLoader = resourceLoader;
       Assert.notNull(primarySources, "PrimarySources must not be null");
       // 将传入的DemoApplication启动类放入primarySources中，这样应用就知道主启动类在哪里，叫什么了
       // SpringBoot一般称呼这种主启动类叫primarySource（主配置资源来源）
       this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources));
       // 3.1 判断当前应用环境
       this.webApplicationType = WebApplicationType.deduceFromClasspath();
       // 3.2 设置初始化器
       setInitializers((Collection) getSpringFactoriesInstances(ApplicationContextInitializer.class));
       // 3.3 设置监听器
       setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
       // 3.4 确定主配置类
       this.mainApplicationClass = deduceMainApplicationClass();
   }
   ```

   自定义SpringApplication

   ```java
   @SpringBootApplication
   public class DemoApplication{
       public static void main(String[] args){
           SpringApplication springApplication = new SpringApplication(DemoApplication.calss);
           springApplication.setWebApplicationType(WebApplicationType.SERVLET); //强制使用WebMvc环境
           springApplication.setBannerMode(Banner.Mode.OFF);//不打印Banner
           springApplication.run(args);
       }
   }
   ```

   WebApplicationType.deduceFromClasspath：判断当前应用环境

   * 从classpath下判断当前SpringBoot应用应该使用哪种环境启动

   setInitializers：设置初始化器

   * setInitializers方法会将一组类型为ApplicationContextInitializer的初始化器放入SpringApplication中

     而这组ApplicationContextInitializer,是在构造方法中，通过getSpringFactoriesInstances得到的

   **ApplicationContextInitializer**

   ```java
   public interface ApplicationContextInitializer<C extends ConfigurableApplicationContext>
   ```

   在IOC容器之前的回调

   SpringFactoriesLoader.loadFactoryNames

   deduceMainApplicationClass：确定主配置类

   ```java
   private Class<?> deduceMainApplicationClass() {
       try {
           StackTraceElement[] stackTrace = new RuntimeException().getStackTrace();
           for (StackTraceElement stackTraceElement : stackTrace) {
               // 从本方法开始往上爬，哪一层调用栈上有main方法，方法对应的类就是主配置类
               if ("main".equals(stackTraceElement.getMethodName())) {
                   return Class.forName(stackTraceElement.getClassName());
               }
           }
       }
       catch (ClassNotFoundException ex) {
           // Swallow and continue
       }
       return null;
   }
   ```

---

1. SpringApplication 的创建和运行时两个不同的步骤
2. SpringBoot会根据当前classpath下的类来决定Web应用类型
3. SpringBoot的应用中包含两个关键组件：ApplicationContextInitializer 和 ApplicationListener，分别是初始化器和监听器，它们都构建SpringApplication 时注册

---

1.SpringApplication 应用中可以使用SpringApplicationRunListener来监听SpringBoot应用的启动过程

2.在创建IOC容器前，SpringApplication会准备运行时环境Environmen

---

三种类型的运行时环境、IOC容器的类型

* **StandardServletEnvironment** - AnnotationConfigServletWebServerApplicationContext
* **Reactive** - StandardReactiveWebEnvironment - AnnotationConfigReactiveWebServerApplicationContext
* **None** - StandardEnvironment - AnnotationConfigApplicationContext

---

SpringFrameWork和SpringBoot中有很多类似与xxx方法和doXXX方法。一般情况下，xxx方法负责引导到doXXX方法，doXXX方法负责真正的逻辑和工作

---

**BeanDefinition**

* BeanDefinition 描述了一个bean实例，该实例具有属性值，构造函数参数值以及具体实现所提供的更多信息
* 这只是一个最小的接口：主要目的是允许BeanFactoryPostProcessor(例如 PropertyPlacehodlerConfigurer) 内省和修改属性值和其他bean元数据

---

1. Banner在初始化允许环境之后，创建IOC容器之前打印
2. SpringApplication 会根据前面确定好的应用类型，创建对应的IOC容器
3. IOC容器在刷新之前会进行初始化、加载主启动类等预处理工作

【至此，主启动类被注册进IOC容器中，IOC容器已经准备好】

---

```java
prepareContext(context, environment, listeners, applicationArgument,printedBanner);
refreshContext(context);
afterRefresh(context,applicationArguments)
```

----

##### 刷新容器-BeanFactory的预处理

###### refershContext

```java
private void refreshContext(ConfigurableApplicationContext context){
    refresh(context);
    if(this.registerShutdownHook){
        try{
            context.registerShutdownHook();
        }catch(AccessControlException ex){
            //Not allowed in some environment
        }
    }
}
```

```java
//最终调到AbstractApplicationContext的refresh方法
public void refresh() throws BeansException, IllegalStateException{
    synchronized(this.startupShutdownMonitor){
        //prepare this context for refreshing
        //1.初始化前的预处理
        prepareRefresh();
        
        //Tell the subclass to refresh the internal bean factory
        //2.获取BeanFactory,加载所有bean的定义信息（未实例化）
        ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();
        
        //Prepare the bean factory for use in this context
        //3.Beanfactory的预处理配置
        prepareBeanFactory(beanfactory);
        
        try{
            //Allows post-processing of the bean factory in context subclasses
            //4.准备BeanFactory完成后进行的后置处理
            postProcessBeanFactory(BeanFactory);
            
            //Invoke factory processors registered as beans in the context
            //5.执行BeanFactory创建后的后置处理器
            invokeBeanFactorypostProcessors(beanFactory);
            
            //Regiter Bean processors that intercept bean creation
            //6.注册Bean的后置处理器
            registerBeanPostProcessors(BeanFactory);
            
            //Initialize message source for this context
            //7.初始化MessageSource
            initMessageSource();
            
            //Initialize event multecaster for this context
            //8.初始化事件派发器
            initApplicationEventMulticaster();
            
            //Initialize other special beans in specific context subclass
            //9.子类的多态onRefresh
            onRefresh();
            
            //check for listener beans and register them
            //10.注册监听器
            registerListeners();
            
            //到此为止 beanFactory已创建完成
            
            //Insrantiate all remaining(non-lazy-init)singletons
            //11.初始化所有剩下的单例bean
            finishBeanFactoryInitialization(beanFactory);
            
            //Last step:public corresponding enent
            //12.完成容器的创建工作
            finishRefresh();
        }
        catch(BeanException ex){
            if(logger.isWarnEnabled()){
                logger.warn("Exception encountered during context initialization -" +
                           "cancelling refresh attempt:" + ex)
            }
            
            //Destroy already created singletons to avoid dangling resources
            destroyBeans();
            
            //Rest 'active' flag
            cancelRefresh(ex);
            
            //Propagate exception to caller
            throw ex;
        }
        finally{
            //Reset common introspection caches in Spring's core,since we
            //might not ever need metadata for singleton beans anymore...
            //13.清除缓存
            resetCommonCaches();
        }
    }
}
```

1. IOC容器在开始刷新之前有加载BeanDefinition的过程
2. BeanFactory 的初始化中会注册后置处理器，和自动注入的支持
3. BeanPostProcessor 的执行时机是在Bean初始化前后执行
4. BeanFactoryPostProcessor的执行时机是所有的BeanDefinition已经被加载，但没有Bean被实例化
5. 包扫描会加载所有BeanDefinition，底层采用递归扫描
6. IOC容器使用ConfigurationClassPostProcessor进行注解组件解析
7. 注册BeanPostProcessor 的时机是 BeanFactory 已经初始化完毕，监听器还没有注册之前
8. 注册ApplicationListener的时机是BeanPostProcesser 注册完，但还没有初始化单例实例Bean
9. IOC容器使用ApplicationEventMulticaster广播事件
10. IOC容器初始化单实例Bean使用getBean方法
11. 真正创建Bean的步骤是doCreateBean
12. 创建Bean的过程
    1. 实例化Bean
    2. 属性赋值&自动注入
    3. 执行初始化方法
13. BeanPostProcessor 中before方法真正的执行时机是在注入之后，初始化方法调用之前

---

IOC容器解决循环依赖的思路

1. 初始化Bean之前，将这个BeanName放入三级缓存
2. 创建Bean将准备创建的Bean放入singletonsCurrentlyInCreation(正在创建的Bean)
3. createNewInstance 方法执行玩后执行 addSingletonFactory ,将这个实例化但没有属性赋值的Bean放入二级缓存，并从三级缓存中移除
4. 属性赋值&自动注入时，引发关联创建
5. 关联创建
   1. 检查“正在被创建的Bean”中是否有即将注入的Bean
   2. 如果有，检查二级缓存中是否有当前创建好但没有赋值初始化的Bean
   3. 如果没有，检查三级缓存中是否有正在创建中的Bean
   4. 至此一般会有，将这个Bean放入二级缓存，并从三级缓存中移除
6. 之后Bean被成功注入，最后执行addSingleton，将这个完全创建好的Bean放入一级缓存，从二级缓存和三级缓存移除，并记录创建了的单实例Bean

---

1. IOC容器初始化完成后会清理缓存
2. SpringBoot对IOC容器的扩展是创建嵌入式Web容器
3. SpringBoot 存在一些版本过时但还没有清理或废弃的组件（如CommandLineRunner 和ApplicationRunner）

---

#### IOC

###### 1.Web应用类型锁定

SpringBoot会根据classpath下存在的类，决定当前应用的类型，以此来创建合适的IOC容器

默认WebMvc环境下，创建IOC容器是AnnotationConfigServletWebServerApplicationContext

###### 2.Sping的SPI技术

springBoot使用SpringFactoriesLoader.loadFactoryNames机制从META-INF/spring.factories文件中读取指定 类/注解，映射的组件全限定类名，以此来反射创建组件。Spring设计的SPI比Java原生的SPI要更灵活，因为它的key可以任意定义类/注解，不再局限于“接口-实现类”的形式

###### 3.SpringApplicationRunListener

SpringApplicationRunListener可以监听SpringApplication的运行方法。通过注册SpringApplicationRunListener，可以自定义的在SpringBoot应用启动过程、运行、销毁时监听对应的时间，来执行自定义逻辑

###### 4.Environment

Spring应用的IOC容器需要依赖Environment - 运行环境，它用来表示整个Spring应用运行时的环境，它分为prifiles 和 properties 两个部分。通过配置不同的profiles，可以支持配置的灵活切换，并且可以同时配置一到多个profile来共同配置Environment

###### 5.多种后置处理器

IOC容器中出现的后置处理器类型非常多

* BeanPostProcesser：Bean实例化后，初始化的前后触发
* BeanDefinitionRegistryPostProcesser：所有Bean的定义信息即将被加载但未实例化时触发
* BeanFactoryPostProcessor：所有的BeanDefinition 已经被加载，但没有Bean被实例化时触发
* InitDestroyAnnotationBeanPostProcessor：触发执行Bean中标注@PostConstruct、@PreDestroy注解的方法
* ConfigurationClassPostProcesser：解析加了@Configuration的配置类，解析@ComponentScan注解扫描的包以及解析@Import、@ImportResource等注解
* AutowiredAnnotationBeanPostProcesser：负责处理@Autowired、@Value等注解

###### 6.监听器与观察者模式

SpringFramework 有原生的 ApplicationListener，在IOC容器中有对应的监听器注册，与事件广播器。通过注册不同的ApplicationListener，并指定事件类型，注册到IOC容器中，IOC容器会自动将其注册并在事件发布时执行监听方法

###### 7.初始化单实例Bean与循环依赖

Bean的初始化经过的步骤非常多，其中也包括AOP的部分。其中IOC容器为了避免出现循环依赖，会在BeanFactory中设计三级缓存来解决setter和@Autowired的循环依赖

###### 8.嵌入式Web容器的创建

SpringBoot扩展的ServletWebServerApplicationContext会在IOC容器的模板方法onRefersh方法中创建嵌入式Web容器

----





