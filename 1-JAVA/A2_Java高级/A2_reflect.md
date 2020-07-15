## Java反射(reflect)

* 类的加载机制：将一个类装在JVM虚拟机中的过程
  1. 加载
  2. 验证（连接）
  3. 准备（连接）为静态成员变量设置默认值
  4. 解释（连接）
  5. 初始化（对类中的静态成分执行初始化，为静态变量赋值，执行静态初始化块）
  6. 使用
  7. 卸载
*  java中有两种成员与对象无关：
  1. 基本数据类型
  2. 静态成员（静态属性、静态方法）
* java中所有类都是Class类实例，Class代表的是在jvm中正在执行的类的实例。
* Class中包含了当前实例的运行信息，内部结构（成员变量、方法、注解、实现接口....）
* **反射**：一个正在运行的类通过反射机制取到类中的具体信息(包含Class实例中)
  * 属性类
  * 构造器类
  * 方法类
  * 注解类
  * 将一个类中的所有成员反射为一个对应的Java类
* 动态绑定：多态

### Class类

* Class类的实例表示正在运行的 Java 应用程序中的类和接口。
* 枚举是一种类，注释是一种接口。
* `Class` 没有公共构造方法。`Class` 对象是在加载类时由 Java 虚拟机以及通过调用类加载器中的  `defineClass` 方法自动构造的。

### Field类

* `Field` 提供有关类或接口的单个字段的信息，以及对它的动态访问权限。反射的字段可能是一个类（静态）字段或实例字段。

### 实例代码

* BeanFactory

  ```java
  /**
   * 解析beans.xml文件，将文件中的信息解析为一个map集合
   * 主键为bean的id
   * 值为bean中class对应的对象
   */
  public class BeanFactory {
  
      //初始化一个map集合存储所有的bean对象
      private  Map<String, Object> beans;
  
      public BeanFactory(String config){
          beans = new HashMap<>();
          //创建解析器
          SAXReader reader = new SAXReader();
  
          try {
              //解析配置文件获取document对象
              Document document = reader.read(new File("src/beans.xml"));
              //搜索所有bean节点
              List<Node> list = document.selectNodes("beans/bean");
              for (Node node : list) {
                  //获取bean节点中的id和class属性值
                  String id = node.valueOf("@id");
                  String clzName = node.valueOf("@class");
                  //加载指定类路径获取Class对象
                  Class clz = Class.forName(clzName);
                  //创建实例
                  Object obj = clz.newInstance();
                  beans.put(id, obj);
  
                  //读取当前节点下的子节点(property 所有属性节点)
                  List<Node> props = node.selectNodes("property");
                  for (Node n : props) {
                      String pname = n.valueOf("@name");
                      String value = n.valueOf("@value");
                      String beanid = n.valueOf("@ref");
  
                      //根据属性名拼接setter方法
                      String methodSet = "set" + pname.substring(0, 1).toUpperCase()+pname.substring(1);
                      //获取setter方法(根据拼接的属性名)
                      Method method = clz.getMethod(methodSet, clz.getDeclaredField(pname).getType());
  
                      if(!"".equals(beanid)){
                          method.invoke(obj, beans.get(beanid));
                      }else{
                          Class c = method.getParameterTypes()[0];
                          method.invoke(obj, TypeUtil.getValue(c.getName(), value));
                      }
                  }
              }
          } catch (DocumentException e) {
              e.printStackTrace();
          }.......
      }
  ```

* ObjectClone

  ```java
  public class ObjectClone {
  
      public static <T> T copy(Object source, Class<T> clz){
          //指定对象的class对象
          //		Class clz = source.getClass();
          //		Class<T> clz = T; 
          T newObj = null;
  
          try {
              //根据class对象创建当前类型的实例(空对象)
              newObj = clz.newInstance();
              //获取当前类中包含的所有属性
              Field [] fields = clz.getDeclaredFields();
              for (Field field : fields) {
                  //拼接获取setter/gettter方法的名称
                  String setMethodName = "set" + field.getName().substring(0, 1).toUpperCase()+field.getName().substring(1);
                  String getMethodName = "get" + field.getName().substring(0, 1).toUpperCase()+field.getName().substring(1);
                  //根据方法名获取方法对象
                  Method method_set = clz.getMethod(setMethodName, field.getType());
                  Method method_get = clz.getMethod(getMethodName);
  
                  //执行源对象指定属性的getter方法，获取属性值
                  Object returnval =  method_get.invoke(source);
                  //执行新对象的setter方法，将源对象的属性值给新对象
                  method_set.invoke(newObj, returnval);
              }
  
          } catch (InstantiationException | IllegalAccessException e) {
              e.printStackTrace();
          }......
          return newObj;
      }
  ```

* TypeUtil

  ```java
  public static Object getValue(String type, String value){
      if("int".equals(type)){
          return Integer.parseInt(value);
      }else if("double".equals(type)){
          return Double.parseDouble(value);
      }else if("float".equals(type)){
          return Float.parseFloat(value);
      }else if("char".equals(type)){
          return value.charAt(0);
      }else if("byte".equals(type)){
          return Byte.parseByte(value);
      }else if("short".equals(type)){
          return Short.parseShort(value);
      }else if("long".equals(type)){
          return Long.parseLong(value);
      }else if("boolean".equals(type)){
          return Boolean.parseBoolean(value);
      }else{
          return value;
      }
  }
  ```

* 建立一个核心Servlet，用其他Servlet来继承该Servlet

  ```java
  //获取客户端请求的方法名称
  String methodName = request.getParameter("method");
  //根据客户端请求的方法名称通过反射机制获取方法对象
  Method method = this.getClass().getMethod(methodName, HttpServletRequest.class, HttpServletResponse.class);
  //执行方法
  method.invoke(this, request,response);
  ```

