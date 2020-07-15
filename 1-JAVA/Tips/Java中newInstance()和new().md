##### Java中的newInstance()和new()

Class.forName()；静态方法的目的是动态加载类

在加载完成后，一般使用newInstance()静态方法来实例化对象以便操作。

为什么加载数据库驱动包的时候没有调用newInstance()?

Class.forName("");的作用是要求JVM查找并加载指定的类，如果在类中有静态初始化器的话，JVM必然会执行该类的静态代码段。而在JDBC规范中明确要求这个Driver类必须向DriverManager注册自己，即任何一个JDBC Driver的Driver类的代码都必须类似如下：

```java
public class MyJDBCDriver implements Driver{
    static{
        Drivermanager.registerDriver(new MyJDBCDriver());
    }
}
```

在静态初始化器中已经进行了注册，所以我们在使用JDBC时只需要Class.forName(XXX.XXX);就可以了

---

Java中工厂模式经常使用newInstance()方法来创建对象，因此从为什么要使用工厂模式上可以找到具体答案。

```java
String className = readFromXMLConfig; //从xml配置文件中获得字符串
class c = Class.forName(className);
factory = (ExampleInterface)c.newInstance();
```

工厂模式的优点是，无论类怎么变化，上述代码不变，甚至可以更换类的兄弟类，只要他们继承ExampleInterface皆可以了。

从JVM的角度看，使用关键字new创建一个类的时候，这个类可以没有被加载。但是使用newInsatance();方法的时候，就必须保证：1.这个类被加载；2.这个类已经连接了。而完成上面两个步骤的正是Class的静态方法forName()所完成的，这个静态方法调用了启动类加载器，即加载Java API的那个加载器。

newInstance()实际上是吧new这个方式分解为两步，即首先调用Class加载方法加载某个类，然后实例化。这样就可以在调用class的静态加载方法forName时获得更好的灵活性，提供了一个降耦的手段。

---

总结：

newInstance：弱类型，低效率，只能调用无参构造

new：强类型，相对高效，能调用任何public构造