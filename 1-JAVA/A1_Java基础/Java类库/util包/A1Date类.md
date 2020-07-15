### Date

* 类 Date 表示特定的瞬间，精确到毫秒。  

* 当前时间

  `Date d = new Date();`

* 年月日时间，年从1900开始计数，月从0到11计数，日不变

  `Date d2 = new Date(97, 6, 19);`

* 年月日时分秒

  `Date d3 = new Date(118, 6, 19, 11, 0, 01);`

* 制定毫秒时间

  `Date d4 = new Date(System.currentTimeMillis());`

* 制定时间的写法

  `Date d5 = new Date("1999/10/10");`

* 以2000-10-10 0:00:00格式打印出时间

  `System.out.println(d5.toLocaleString());`

* 一周中的第几天，从0开始计数

  `d.getDay();`

* 一个月中的第几天，几号

  ` d.getDate();`

* 返回一个毫秒时间

  `long l = Date.parse("2000/01/01");`

* 比较时间(返回boolean类型)

  ```java
  Date d = new Date();
  Date d2 = new Date(117, 6, 19);
  System.out.println(d.after(d2));//true
  System.out.println(d.before(d2));//false
  System.out.println(d.equals(d2));//false
  ```

* Timestamp

  * 时间戳













