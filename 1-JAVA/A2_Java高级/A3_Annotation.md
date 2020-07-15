### 注解

* Annotation 是 JDK1.5之后出的功能，在不影响Java源代码的前提下，通过标注的方式在类、属性、方法上添加注释，然后通过一些工具(比如反射)对其进行解析，然后实现特定功能。

* JDK1.5中常见内置注解

  **@Override**：标注方法是从父类重写的方法，检查方法是否为重写的方法

  **@Deprecated**：标注在任何地方，表示被标注的元素为过时

  **@SuppressWarnings()**：忽略编译期间的警告(泛型未定义、变量未使用)

#### 自定义注解

* public @interface 注解名{

  }

* JDK1.5也为注解提供用于修饰注解的元注解，这些注解只能用于修饰注解而不能使用与非注解对象。

  * **@Retention ** 注解的保留位置(source，class，runtime)

    * RetentionPolicy.RUNTIME：编译器会将注释保留在字节码文件中，同时在JVM中进行保留，所以可以通过反射机制读取到注解信息
    * RetentionPolicy.CLASS：编译器会将该注释保留在字节码文件中，但是不会保留在JVM中，所以无法通过反射机制读取到注解信息
    * RetentionPolicy.SOURCE：该注解只在源代码中保留，作为标记使用

  * **@Target**：声明注解可以使用的位置，可选属性有以下：

    * ElementType.TYPE：可修饰类、接口
    * ElementType.FIELD：可修饰属性(全局变量)
    * ElementType.CONSTRUCTOR：可修饰构造器
    * ElementType.METHOD：可方法修饰
    * ElementType.PACKAGE：可修饰包
    * ElementType.PARAMETER：可修饰参数
    * ElementType.LOCAL_VARIABLE：可修饰局部变量

    > 当存在多个时：@Target({ElementType.METHOD,ElementType.FIELD})

  * **@Documented**：如果需要将注解信息生成到javadoc时可以在注解上加入该注解

  * **@Inherited**：使用该注解的元素被子类继承时能够同时继承该注解