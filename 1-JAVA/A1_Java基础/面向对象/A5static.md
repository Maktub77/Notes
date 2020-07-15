### Static

* 可以修饰属性、方法、代码块

  1. 修饰属性时，表示该属性是一个**类属性**，所以通过该类new出的对象公用这个属性

  2. 修饰方法时，为了方便调用，调用方式：类名.方法名

     * static :加载访问修饰符与返回类型之间

       作用：为了方便调用，调用时：类名.方法名【属性名】不需要new

  3. 修饰代码块时，该代码自动调用，且调用一次                                                                                                     

     > 静态代码块：当类第一次被加载时，静态代码块机会自动运行；静态代码块只执行一次

* 静态方法：只能调用其他的**静态属性**和**静态的方法**

```JAVA
public static void run(){
    //		System.out.println( name); //静态方法不能调用非静态属性
    System.out.println( age );  //静态方法可以调用静态属性
    //		System.out.println( say(0));//静态方法不能调用非静态方法
    System.out.println( count(3) );//静态方法可以调用静态方法
}
```

* 非静态方法：可以调用非静态属性的属性和方法，也可以调用静态的属性和方法

```java 
//非静态方法: 可以调用非静态属性的属性和方法，也可以调用静态的属性和方法 
public int count2(){
    System.out.println( name);
    System.out.println( age ); 
    System.out.println( say(0));
    System.out.println( count(3) );
    return 1;
```

### 静态代码块与无参构造函数

* 当new多个对象时静态代码块仅执行一次，无参构造函数new几次，执行几次
* 当有多个静态代码块时，多个静态代码块逐个执行，且只执行一次

