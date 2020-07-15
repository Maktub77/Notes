### final关键字

* 最终的意思
  1. 修饰变量，表示常量，值不发生改变
  2. 修饰方法，表示该方法不能被子类重写，但可以被重载
  3. 修饰类：表示该类是一个最终类，不能被继承

```java
public class Test01 {
    final int AGE = 18;//局部变量
}
final class People{	// final 修饰类，不能被继承
    
    final int AGE = 18;//全局变量

    public final void eat(){//final 修饰方法，不能被子类重写
        System.out.println("father很会吃...");
    }
}
```

