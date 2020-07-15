#### `StringBuffer`

* 追加`append()`

  ```java
  //定长
  String str = "abc";
  str.concat("def");
  System.out.println( str );//abc
  
  //变长
  StringBuffer strb = new StringBuffer("abc");
  strb.append("def");//追加
  System.out.println( strb );//abcdef
  ```

* 删除`delete()`

  ```java
  //删除包括开始位置，不包括结束位置
  strb.delete(3, 5);
  System.out.println( strb );
  ```

* 插入`insert()`

  ```java
  //从第4个位置开始插入
  strb.insert(4, "^_^");
  System.out.println( strb );
  ```

* 翻转`reverse()`

  ```java
  StringBuffer sb = new StringBuffer("I Love You");
  sb.reverse();
  System.out.println( sb );
  ```

#### `String` 、`StringBuffer`、`StringBuilder`的区别

* String（出生于 `JDK1.0`）不可变字符序列
  不可变是因为，在底层有一个final修饰的char[]数组

* `StringBuffer`(出生于`JDK1.0`)  线程安全的可变字符序列

* `StringBuilder`(出生于`JDK1.5`)  非线程安全的可变字符序列

  用于替换`StringBuffer`，因为速度快

