### String

* ```java
  String str1 = "abc";
  //在内存创建了三个东西，两个对象
  String str2 = new String("abc");
  ```

* ```java
  String str3 = new String();
  String str4 = "";
  ```

* `System.out.println( str1.equals(str2));`

* char数组类型转换为String类型

  ```java
  char [] arr = {'a', 'b', 'c', 'd'};	
  String str = new String (arr);
  System.out.println(str);
  ```

* byte数组转换成String

  ```java
  byte [] bb = {23, 45, 21, 32, 34};
  String str1 = new String(bb);
  System.out.println( str1);
  ```

* ```java
  String str1 = "abc";//定长
  System.out.println( str1.length() );//返回字符串的长度
  
  str1 = "abc d";
  System.out.println( str1.length() );
  ```

* ```java
  byte [] arr = {56, 67, 90};
  String str2 = new String (arr, 1, 2);//索引从第一个开始截取两个
  System.out.println( str2 );
  
  char [] cc ={'a', 'v', 'n', 'm', 's'};//索引从第二个开始截取三个
  String str3 = new String (cc, 2, 3);
  ```

#### `indexOf()`

```java
String str = "abcdefghijklmnabcdef";

str.indexOf('d');//在字符串中第一次出现的索引
str.indexOf('d', 11);//跳过11个字符，再查找d的索引

String str1 = "def";
str.indexOf(str1);//在一个字符串中查找子字符串的位置

str.charAt(str.length()-1);//提取字符串中的最后一个字符并转换为char类型
```

#### `substring()`

```java
String str = "absshs我爱你中国";

//从第4个索引处开始（包括第4个索引），之后的所有字符串组成一个新的字符串
String s1 = str.substring(4);
System.out.println( s1 );//hs我爱你中国

//从第6个索引处开始，到第6个索引处结束，组成一个新的字符串(包括左边，不包括右边)
String s2 = str.substring(4, 6); 
System.out.println( s2 );//hs
```

#### `concat()`字符串的拼接

```java
String str2 = "abc";
String str3 = "def";		
String s3 = str2.concat(str3);// str1 + str2		
System.out.println( s3 );
```

#### `replace()`字符串的替换

```java
String str4 = "aaadeafdessaa";

String s4 = str4.replace('a', 'm');
System.out.println( s4 );

s4 = str4.replace("aa", "mm");
System.out.println( s4 );
```

* ```java
  //比较字符串:字符串相等返回0，字符串s3大于s4返回正数，字符串s3小于s4返回负数
  String s3 = "abcdef";
  String s4 = "abcdef";
  System.out.println( s3.compareTo(s4));
  ```

* ```java
  //判断字符串的开头是否为你规定的字符
  System.out.println( s3.startsWith("abc"));
  
  //判断字符串的结尾是否为你规定的字符
  System.out.println( s3.endsWith("ef"));
  ```

* ```java
  //取消字符串最前面和最后面的空格
  String s5 = "   a b c ";
  System.out.println( s5.trim() );
  ```

* ```java
  String s6 = "SAfadAAf";
  //将字符串全转换为大写
  System.out.println( s6.toUpperCase());
  //将字符串全转换为小写
  System.out.println( s6.toLowerCase());
  ```

* String类型转char类型`toCharArray()`

  ```java
  //String 转char类型
  char c = 'a';
  String s1 = c + "";
  System.out.println( s1 );
  
  char [] cc = {c};
  String s2 = new String(cc);
  System.out.println( s2 );
  ```

  ```java
  //String 转char类型数组
  String str1 = "abcdef";	
  for(int i = 0; i < str1.length()-1; i++){
      char cc2 = str1.charAt(i);
  }
  
  char [] cc3 = str1.toCharArray();
  for(int i = 0; i < cc3.length; i++){
      System.out.print( cc3[i] );
  }
  ```

