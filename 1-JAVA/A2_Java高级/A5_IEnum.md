## 通用枚举扫描并自动关联注入

1. 自定义枚举实现IEnum接口，申明自动注入为通用枚举转换处理器

   ```java
   //枚举属性，必须实现IEnum接口如下：
   public enum AgeEnum implements IEnum {
       ONE(1, "一岁"),
       TWO(2, "二岁");
   
       private int value;
       private String desc;
   
       AgeEnum(final int value, final String desc) {
           this.value = value;
           this.desc = desc;
       }
   
       @Override
       public Serializable getValue() {
           return this.value;
       }
   
       @JsonValue // Jackson 注解，可序列化该属性为中文描述【可无】
       public String getDesc(){
           return this.desc;
       }
   }
   ```

2. 配置扫描通用枚举

   * 注：spring MVC配置参考，安装集成`MybatisSqlSessionFactoryBean`枚举包扫描，spring boot例子配置如下：

   * `resource/application.yml`

     ```yml
     mybatis-plus:
         # 支持统配符 * 或者 ; 分割
         typeEnumsPackage: com.baomidou.springboot.entity.enums
       ....
     ```
