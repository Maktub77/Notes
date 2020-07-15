### 泛型

* 泛型:定义使用"<>"符号，在尖括号里面写上集合存放数据的数据类型（引用类型） 

* 泛型的作用：用来约束集合中存放数据的类型

  ```java
  ArrayList<String> list = new ArrayList<String>();
  
  HashSet<String> set = new HashSet<String>();
  
  HashMap<Integer, String> map = new HashMap<Integer, String>();
  ```

### foreach

* foreach 不需要指定循环次数，for后的小括号中一个":"

*  冒号后面的变量表示数据源，冒号前面是foreach循环接受数据的变量，该变量的类型由数据源决定

*  foreach类型的迭代器，没有下标，依次拿下一个元素，直到拿完为止，循环自动结束，可以防止越界问题

*  foreach循环只能输出，不能赋值

  ```java
  for(int n:arr){
      System.out.print(n);
  }
  ```

* foreach一般跟set集合使用，弥补了set集合没有get()方法的缺陷

  ```java
  HashSet set = new HashSet();
  
  for (int i = 0; i < 10; i++) {
      set.add("abc" + i);
  }
  for(Object o : set){
      String str = (String)o;
      System.out.println(o);
  }
  ```

  