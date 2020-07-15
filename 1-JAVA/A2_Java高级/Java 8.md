## JAVA8

* **Lambda表达式**：Lambda允许把函数作为一个方法的参数
* **方法引用**：可以直接引用已有Java类或对象（实例）的方法或构造器。与lambda联合引用。
* **默认方法**：默认方法就是一个在接口里面有了一个实现的方法
* **新工具**：新的编译工具，如：Nashorn引擎jjs、类依赖分析器jdeps
* **Stream API**：新添加的Stream API(java.util.stream)把真正的函数式编程风格引入到Java中
* **Date Time API**：加强对日期与时间的处理
* **Optional类**：Optional类已经成为Java类库的一部分，用来解决空指针异常
* **Nashorn，JavaScript引擎**：Java8提供了一个新的Nashorn javascript引擎，它允许我们在JVm上运行特定的JavaScript应用

----

### Lambda表达式

* 语法格式：

  ```java
  (parameters) -> expression
  或
  (parameters) ->{ statements; }
  ```

* 特征：

  * **可选类型声明：**不需要声明参数类型，编译器可以统一识别参数值
  * **可选的参数圆括号：**一个参数无需定义圆括号，但多个参数需要定义圆括号
  * **可选的大括号：**如果主体包含了一个语句，就不需要使用大括号
  * **可选的返回关键字：**如果主体只有一个表达式返回值则编译器会自动返回值，大括号需要指定表达式返回一个数值

* 实例：

  ```java
  // 1. 不需要参数,返回值为 5  
  () -> 5  
    
  // 2. 接收一个参数(数字类型),返回其2倍的值  
  x -> 2 * x  
    
  // 3. 接受2个参数(数字),并返回他们的差值  
  (x, y) -> x – y  
    
  // 4. 接收2个int型整数,返回他们的和  
  (int x, int y) -> x + y  
    
  // 5. 接受一个 string 对象,并在控制台打印,不返回任何值(看起来像是返回void)  
  (String s) -> System.out.print(s)
  ```


注意：

* Lambda表达式主要用来定义行内执行的方法类型接口。
* Lambda表达式免去了使用匿名方法的麻烦，并且给予Java简单但是强大的函数化的编程能力。

#### 变量作用域

* Lambda表达式只能引用标记了final的外层局部变量，这就是说不能再lambda内部修改定义在域外的局部变量，否则会编译错误。
* Lambda表达式的局部变量可以不用声明为final，但是必须不可被后面的代码修改（即隐性的具有final的语义）
* 在Lambda表达式当中不允许声明一个与局部变量同名的参数或者局部变量

----

### 方法引用

* 通过方法的名字来指向一个方法
* 可以使语言的构造更紧凑简洁，减少冗余代码
* 使用一对冒号`::`

四种方法引用类型

| **类型**                         | **示例**                             |
| -------------------------------- | ------------------------------------ |
| 引用静态方法                     | ContainingClass::staticMethodName    |
| 引用某个对象的实例方法           | containingObject::instanceMethodName |
| 引用某个类型的任意对象的实例方法 | ContainingType::methodName           |
| 引用构造方法                     | ClassName::new                       |

----

### 函数式接口

* 一个有且仅有一个抽象方法，但是可以有多个非抽象方法的接口。

* 主要用在Lambda表达式和方法引用（也可认为是Lambda表达式）上

* 定义一个函数式接口

  ```Java
  @FunctionalInterface
  interface GreetingService{
      void sayMessage(String message);
  }
  ```

* 可以使用Lambda表达式来表示接口的一个实现（**Java8之前一般使用匿名类实现**）

* `@FunctionalInterface`注解

  * 用于**编译级错误检查**，加上该注解，当接口不符合函数式接口定义的时候，编译器会报错

---

### 默认方法

* 接口可以有实现的方法，而且不需要实现类去实现其方法

* 只需要在方法名前面加个`default`关键字即可实现默认方法

  >**为什么要有这个特性？** 
  >
  >​	接口好处是面向抽象而不是面向具体编程
  >
  >​	缺陷是，当需要修改接口的时候，需要修改全部实现该接口的类。

* 解决接口的修改与现有的实现不兼容的问题。

#### 语法

```java
public interface Vehicle{
    dedault void print(){
        System.out.println("我是一辆车！");
    }
}
```

#### 多个默认方法

* 一个接口有默认方法，考虑这样的情况，一个类实现多个接口，且这些接口有相同的默认方法

  ```java
  public interface Vehicle {
     default void print(){
        System.out.println("我是一辆车!");
     }
  }
   
  public interface FourWheeler {
     default void print(){
        System.out.println("我是一辆四轮车!");
     }
  }
  ```

* 创建自己的默认方法，来覆盖重写接口的默认方法

  ```java
  public class Car implements Vehicle, FourWheeler {
     default void print(){
        System.out.println("我是一辆四轮汽车!");
     }
  }
  ```

* 使用super来调用指定接口的默认方法

  ```java
  public class Car implements Vehicle, FourWheeler {
     public void print(){
        Vehicle.super.print();
     }
  }
  ```

---

### Stream

* 将要处理的元素集合看作一种流，流在管道中传输，并且可以在管道的节点上进行处理，比如筛选，排序，聚合等。

* 元素流在管道中经过中间操作（intermediate operation）的处理，最后由最终操作（terminal operation）得到前面处理的结果。

  ```
  +--------------------+       +------+   +------+   +---+   +-------+
  | stream of elements +-----> |filter+-> |sorted+-> |map+-> |collect|
  +--------------------+       +------+   +------+   +---+   +-------+
  ```

* 以上流转换为Java代码：

  ```java
  List<Integer> transactionsIds = widgets.stream()
      .filter(b -> b.getColor() == RED)
      .sorted((x,y) -> x.getWeight - y.getWeight))
      .mapToInt(Widget::getWeight)
      .sum();
  ```

#### 什么是Stream?

Stream（流）是一个来自数据源的元素队列并支持聚合操作

* 元素是特定类型的对象，形成一个队列。 Java中的Stream并不会存储元素，而是按需计算。
* **数据源** 流的来源。 可以是集合，数组，I/O channel， 产生器generator 等。
* **聚合操作** 类似SQL语句一样的操作， 比如filter, map, reduce, find, match, sorted等。

和以前的Collection操作不同， Stream操作还有两个基础的特征：

* **Pipelining**: 中间操作都会返回流对象本身。 这样多个操作可以串联成一个管道， 如同流式风格（fluent style）。 这样做可以对操作进行优化， 比如延迟执行(laziness)和短路( short-circuiting)。
* **内部迭代**： 以前对集合遍历都是通过Iterator或者For-Each的方式, 显式的在集合外部进行迭代， 这叫做外部迭代。 Stream提供了内部迭代的方式， 通过访问者模式(Visitor)实现。

#### 生成流

* 在 Java 8 中, 集合接口有两个方法来生成流：
  - **stream()** − 为集合创建串行流。
  - **parallelStream()** − 为集合创建并行流。

```java
List<String> strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl"); List<String> filtered = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.toList());
```

#### forEach

* Stream 提供了新的方法 'forEach' 来迭代流中的每个数据。以下代码片段使用 forEach 输出了10个随机数：

  ```java
  Random random = new Random();
  random.ints().limit(10).forEach(System.out::println);
  ```

#### map

* map 方法用于映射每个元素到对应的结果，以下代码片段使用 map 输出了元素对应的平方数：

  ```java
  List<Integer> numbers = Arrays.asList(3,2,3,7,5,2); //获取对应的平方数
  List<Integer> squaresList = numbers.stream().map(i->i*i).distinct.collect(Collectors.toList());
  ```

#### filter

* filter 方法用于通过设置的条件过滤出元素。以下代码片段使用 filter 方法过滤出空字符串：

  ```java
  List<String> strings = Arrays.asList("aa","","s","Dsa"); //获取空字符串的数量
  int count = Strings.stream().filter(strings -> string.isEmpty()).count();
  ```

#### limit

* limit 方法用于获取指定数量的流。 以下代码片段使用 limit 方法打印出 10 条数据：

  ```java
  Random random = new Random();
  random.ins().limit(10).forEach(System.out::println);
  ```

#### sorted

* sorted 方法用于对流进行排序。以下代码片段使用 sorted 方法对输出的 10 个随机数进行排序：

  ```java
  Random random = new Random();
  random.ints()
      .limit(10)
      .sorted()
      .forEach(System.out::println);
  ```

#### 并行（parallel）程序

* parallelStream 是流并行处理程序的代替方法。以下实例我们使用 parallelStream 来输出空字符串的数量：

  ```java
  List<String> strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");//获取空字符串的数量
  int count = strings.parallelStream()
      .filter(string->string
      .isEmpty()).count;
  ```

我们可以很容易的在顺序运行和并行直接切换。

#### Collectors

* Collectors 类实现了很多归约操作，例如将流转换成集合和聚合元素。Collectors 可用于返回列表或字符串：

  ```java
  List<String>strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl"); 
  List<String> filtered = strings.stream()
      .filter(string -> !strings
      .isEmpty())
      .collect(Collectors.toList());
  System.out.println ("筛选列表："+filtered);
  
  String mergedString = strings.stream()
      .filter(string -> !string.isEmpty())
      .collect(Collectors.joining(", ")); 
  
  System.out.println("合并字符串: " + mergedString);
  ```

#### 统计

* 另外，一些产生统计结果的收集器也非常有用。它们主要用于int、double、long等基本类型上，它们可以用来产生类似如下的统计结果。

  ```java
  List<Integer> numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);   
  IntSummaryStatistics stats = numbers.stream()
      .mapToInt((x) -> x)
      .summaryStatistics();  
  System.out.println("列表中最大的数 : " + stats.getMax()); System.out.println("列表中最小的数 : " + stats.getMin()); System.out.println("所有数之和 : " + stats.getSum()); System.out.println("平均数 : " + stats.getAverage());
  ```

---

### Optional类

* 一个可以为null的容器对象。如果值存在则isPresent()方法会返回true，调用get()方法会返回该对象。
* Optional是个容器：它可以保存类型T的值，或者仅仅保存null。Optional提供很多有用的方法，这样我们就不用显式进行空值检测。
* 很好的解决空指针异常。

#### 类声明

```java
public final class optional<T> extends Object{}
```

#### 类方法

| 序号 | 方法 & 描述                                                  |
| ---- | ------------------------------------------------------------ |
| 1    | static <T> Optional<T> empty()<br>返回空的 Optional 实例。   |
| 2    | boolean equals(Object obj)<br>判断其他对象是否等于 Optional。 |
| 3    | Optional<T> filter(Predicate<? super <T> predicate)<br>如果值存在，并且这个值匹配给定的 predicate，返回一个Optional用以描述这个值，否则返回一个空的Optional。 |
| 4    | <U> Optional<U> flatMap(Function<? super T,Optional<U>> mapper)<br>如果值存在，返回基于Optional包含的映射方法的值，否则返回一个空的Optional |
| 5    | T get()<br>如果在这个Optional中包含这个值，返回值，否则抛出异常：NoSuchElementException |
| 6    | int hashCode()<br>返回存在值的哈希码，如果值不存在 返回 0。  |
| 7    | void ifPresent(Consumer<? super T> consumer)<br>如果值存在则使用该值调用 consumer , 否则不做任何事情。 |
| 8    | boolean isPresent()<br>值存在则方法会返回true，否则返回 false。 |
| 9    | Optional<U> map(Function<? super T,? extends U> mapper)<br>如果有值，则对其执行调用映射函数得到返回值。如果返回值不为 null，则创建包含映射返回值的Optional作为map方法返回值，否则返回空Optional。 |
| 10   | static <T> Optional<T> of(T value)<br>返回一个指定非null值的Optional。 |
| 11   | static <T> Optional<T> ofNullable(T value)<br>如果为非空，返回 Optional 描述的指定值，否则返回空的 Optional。 |
| 12   | T orElse(T other)<br>如果存在该值，返回值， 否则返回 other。 |
| 13   | T orElseGet(Supplier<? extends T> other)<br>如果存在该值，返回值， 否则触发 other，并返回 other 调用的结果。 |
| 14   | <X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier)<br>如果存在该值，返回包含的值，否则抛出由 Supplier 继承的异常 |
| 15   | String toString()<br>返回一个Optional的非空字符串，用来调试  |

**注意：** 这些方法是从 **java.lang.Object** 类继承来的。

```java
import java.util.Optional;

public class Java8Tester {
    public static void main(String args[]){

        Java8Tester java8Tester = new Java8Tester();
        Integer value1 = null;
        Integer value2 = new Integer(10);

        // Optional.ofNullable - 允许传递为 null 参数
        Optional<Integer> a = Optional.ofNullable(value1);

        // Optional.of - 如果传递的参数是 null，抛出异常 NullPointerException
        Optional<Integer> b = Optional.of(value2);
        System.out.println(java8Tester.sum(a,b));
    }

    public Integer sum(Optional<Integer> a, Optional<Integer> b){

        // Optional.isPresent - 判断值是否存在

        System.out.println("第一个参数值存在: " + a.isPresent());
        System.out.println("第二个参数值存在: " + b.isPresent());

        // Optional.orElse - 如果值存在，返回它，否则返回默认值
        Integer value1 = a.orElse(new Integer(0));

        //Optional.get - 获取值，值需要存在
        Integer value2 = b.get();
        return value1 + value2;
    }
}
```

