### System

* 输出字符为红色`System.err.println("aaa");s`

* `arraycopy`

  ```jav
  int [] arr1 = {1, 2, 3, 4, 5, 6, 7, 8, 9};
  
  int [] arr2 =new int[10];
  //从原数组（arr1）的第2个索引开始复制到目标数组（arr2）的第0个索引处开始，复制4个
  System.arraycopy(arr1, 2, arr2, 0, 4);
  System.out.println(Arrays.toString(arr2));
  ```

* `console`拿到dos控制台，不是Eclipse的控制台

  ```java
  package com.softeem05.system;
  import java.io.Console;//需要导入io包
  public class Test03 {
      public static void main(String[] args) {
          //拿到dos控制台，不是Eclipse的控制台
          System.out.println("请输入：");
          Console c = System.console();
          String str = c.readLine();
          System.out.println("str-->" + str);
      }
  }
  ```

* `currentTimeMillis()`返回以毫秒为单位的当前时间

  > 返回：当前时间与协调世界时1970年1月1日午夜之间的时间差（以毫秒为单位测量）。

  ```java
  long l = System.currentTimeMillis();
  
  for (int i = 0; i < 10000; i++) {
      int k = 0;
  }
  long l2= System.currentTimeMillis();
  System.out.println(l2-l);//该循环运行所需的时间
  }
  ```

* void 与 return 一起使用时，return后什么都不跟，表示结束方法，返回到此方法的调用的地方

  ```java
  public static void a(){
      int i = 0;
      if(i == 1){
          System.exit(0);
      }else{
          return;
      }
  }
  ```