---
https://beanvalidation.org/
---

## Bean Validation

Bean Validation是一个Java规范

* 允许通过注释表达对象模型的约束
* 允许以可扩展的方式编写自定义约束
* 提供API来验证对象和对象图
* 提供API以验证参数并返回方法和构造参数的值
* 报告违法行为（本地化）
* 在Java SE上运行，但集成在Java EE6及更高版本中; Bean Validation 2.0是Java EE 8的一部分 

```java
public class User {

    private String email;

    @NotNull @Email
    public String getEmail() {
      return email;
    }

    public void setEmail(String email) {
      this.email = email;
    }
}

public class UserService {

  public void createUser(@Email String email,
                         @NotNull String name) {
    ...
  }
}
```

> 运行一次，约束任何地方

### 约束注解的定义

1. 约束注解 

   ### Bean Validation规范内嵌的约束注解定义

   | **约束注解名称** | **约束注解说明**                                             |
   | ---------------- | ------------------------------------------------------------ |
   | @Null            | 验证对象是否为空                                             |
   | @NotNull         | 验证对象是否为非空                                           |
   | @AssertTrue      | 验证 Boolean 对象是否为 true                                 |
   | @AssertFalse     | 验证 Boolean 对象是否为 false                                |
   | @Min             | 验证 Number 和 String 对象是否大等于指定的值                 |
   | @Max             | 验证 Number 和 String 对象是否小等于指定的值                 |
   | @DecimalMin      | 验证 Number 和 String 对象是否大等于指定的值，小数存在精度   |
   | @DecimalMax      | 验证 Number 和 String 对象是否小等于指定的值，小数存在精度   |
   | @Size            | 验证对象（Array,Collection,Map,String）长度是否在给定的范围之内 |
   | @Digits          | 验证 Number 和 String 的构成是否合法                         |
   | @Past            | 验证 Date 和 Calendar 对象是否在当前时间之前                 |
   | @Future          | 验证 Date 和 Calendar 对象是否在当前时间之后                 |
   | @Pattern         | 验证 String 对象是否符合正则表达式的规则                     |

* 约束注解和普通的注解一样，一个典型的约束注解的定义应该至少包括如下内容

  ```java
  @Target({ METHOD, FIELD, ANNOTATION_TYPE, CONSTRUCTOR, PARAMETER })   // 约束注解应用的目标元素类型 METHOD, FIELD, ANNOTATION_TYPE, CONSTRUCTOR, PARAMETER
  @Retention( RUNTIME )   // 约束注解应用的时机 RUNTIME
  @Constraint(validatedBy ={NotEmptyValidator.class})  // 与约束注解关联的验证器 NotEmptyValidator.class
  public @interface ConstraintName{ 
      String message() default " this string may be empty ";   // 约束注解验证时的输出消息
      Class<?>[] groups() default { };  // 约束注解在验证时所属的组别
      Class<? extends Payload>[] payload() default { }; // 约束注解的有效负载
  }
  ```

* 约束验证器:用来验证具体的Java Bean是否满足该约束注解声明的条件

  ```java
  public class NotEmptyValidator implements ConstraintValidator<NotEmpty, String>{ 
      public void initialize(NotEmpty parameters) { 
      } 
      public boolean isValid(String string, 
                             ConstraintValidatorContext constraintValidatorContext) { 
          if (string == null) return false; 
          else if(string.length()<1) return false; 
          else return true; 
      } 
  }
  ```

### 约束验证规则（约束验证器）

* Bean Validation 规范对 Java Bean 的验证流程如下：在实际使用中调用 **Validator.validate(*JavaBeanInstance*)** 方法后，Bean Validation 会查找在 *JavaBeanInstance*上所有的约束声明，对每一个约束调用对应的约束验证器进行验证，最后的结果由约束验证器的 **isValid** 方法产生，如果该方法返回 true，则约束验证成功，否则验证失败。验证失败的约束将产生约束违规对象（ConstraintViolation 的实例）并放到约束违规列表中。验证完成后所有的验证失败信息均能在该列表中查找并输出
* 前提条件：
  * Bean Validation 规范规定在对 Java Bean 进行约束验证前，目标元素必须满足以下条件：
    -  如果验证的是属性（getter 方法），那么必须遵从 Java Bean 的命名习惯（JavaBeans 规范）；
    -  静态的字段和方法不能进行约束验证；
    -  约束适用于接口和基类；
    -  约束注解定义的目标元素可以是字段、属性或者类型等；
    -  可以在类或者接口上使用约束验证，它将对该类或实现该接口的实例进行状态验证；
    -  字段和属性均可以使用约束验证，但是不能将相同的约束重复声明在字段和相关属性（字段的 getter 方法）上。

### 约束注解的声明

### 约束验证流程

## Bean Validation 规范接口及其可扩展的实现

1. Bootstrapping 相关接口 			  

   Bootstrapping 相关接口提供 ValidatorFactory 对象，该对象负责创建 Validator（验证器）实例，该实例即是 Bean Validation 客户端用来进行约束验证的主体类。Bootstrapping 相关接口主要包括 5 类，如表 2 所示：

   表 2. Bootstrapping 相关接口及其作用

   | **接口**                                    | **作用**                                                     |
   | ------------------------------------------- | ------------------------------------------------------------ |
   | javax.validation.validation                 | Bean Validation 规范的 API 默认提供该类，是整个 API 的入口，用来产生 Configuraton 对象实例，并启动环境中 ValidationProvider 的具体实现。 |
   | javax.validation.ValidationProviderResolver | 返回执行上下文环境中所有的 BeanValidationProviders 的列表，并对每一个 BeanValidationProvider 产生一个对象实例。BeanValidation 规范提供一个默认的实现。 |
   | javax.validation.spi.ValidationProvider     | 具体的 BeanValidationProvider 实现需要实现该接口。该接口用来生成具体的 Congfiguration 接口的实现。 |
   | javax.validation.Configuration              | 收集上下文环境中的配置信息，主要用来计算如何给定正确的 ValidationProvider，并将其委派给 ValidatorFactory 对象。 |
   | javax.validation.ValidatorFactory           | 从一个具体的 BeanValidationProvider 中构建 Validator 的实例。 |

	.   Validator 接口 			  

   该接口（javax.validation.Validator）定义了验证实例的方法，主要包括三种，如表 2 所示：

   表 3. Validator 接口中的方法及其作用

| **方法名**                                                   | **作用**                               |
| ------------------------------------------------------------ | -------------------------------------- |
| <T> Set<ConstraintViolation<T>> validate(T object, Class<?>... groups) | 该方法用于验证一个给定的对象           |
| <T>Set<ConstraintViolation<T>> validateProperty(T object, String propertyName, Class<?>...groups) | 该方法用于验证给定对象中的字段或者属性 |
| <T> Set<ConstraintViolation<T>> validateValue(Class<T> beanType, String propertyName, Object value, Class<?>... groups) | 该方法用于验证给定对象中的属性的具体值 |



