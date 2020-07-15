# [Lombok](https://blog.csdn.net/qq_36314960/article/details/79565899)

* Lombok项目是一种自动接通你的编辑器和构建工具的一个Java库.
* lombok其实就是帮助我们编写getter或者equals方法的一个“工具”,当我们的字段发生改变时，lombok也会对相应的getter方法进行改变。

优点：

* 简化冗余的JavaBean代码
* 大大提高JavaBean中方法的执行效率

## Lombok注解详解

@Data注解：在JavaBean或类JavaBean中使用，这个注解包含范围最广，它包含getter、setter、NoArgsConstructor注解，即当使用当前注解时，会自动生成包含的所有方法；

@getter注解：在JavaBean或类JavaBean中使用，使用此注解会生成对应的getter方法；

@setter注解：在JavaBean或类JavaBean中使用，使用此注解会生成对应的setter方法；

@NoArgsConstructor注解：在JavaBean或类JavaBean中使用，使用此注解会生成对应的无参构造方法；

@AllArgsConstructor注解：在JavaBean或类JavaBean中使用，使用此注解会生成对应的有参构造方法；

@ToString注解：在JavaBean或类JavaBean中使用，使用此注解会自动重写对应的toStirng方法；

@EqualsAndHashCode注解：在JavaBean或类JavaBean中使用，使用此注解会自动重写对应的equals方法和hashCode方法；

@Slf4j：在需要打印日志的类中使用，当项目中使用了slf4j打印日志框架时使用该注解，会简化日志的打印流程，只需调用info方法即可；

@Log4j：在需要打印日志的类中使用，当项目中使用了log4j打印日志框架时使用该注解，会简化日志的打印流程，只需调用info方法即可；

在使用以上注解需要处理参数时，处理方法如下（以@ToString注解为例，其他注解同@ToString注解）：

@ToString(exclude="column")

意义：排除column列所对应的元素，即在生成toString方法时不包含column参数；

@ToString(exclude={"column1","column2"})

意义：排除多个column列所对应的元素，其中间用英文状态下的逗号进行分割，即在生成toString方法时不包含多个column参数；

@ToString(of="column")

意义：只生成包含column列所对应的元素的参数的toString方法，即在生成toString方法时只包含column参数；；

@ToString(of={"column1","column2"})

意义：只生成包含多个column列所对应的元素的参数的toString方法，其中间用英文状态下的逗号进行分割，即在生成toString方法时只包含多个column参数；

## 使用Lombok可能需要注意的地方

* 你的IDE是Idea时，要注意你的Idea是支持Lombok的，如果不支持请更换2017版本尝试。
* 使用Lombok时，你的编辑器可能会报错，这时请在你的IDE中安装Lombok插件（如果使用的Idea则直接搜索Lombok插件，选择星级最高的，直接安装就是，其他Ide类同）。
* 参数的处理往往都是根据项目需求来进行，请妥善处理参数。

> 注：无法支持多种参数构造器的重载