### Calender

* Calendar类是一个抽象类。

```java
Calendar c = Calendar.getInstance();
//当前月份
System.out.println(c.get(Calendar.DAY_OF_MONTH));
//当前周
System.out.println(c.get(Calendar.DAY_OF_WEEK));
//当前日
System.out.println(c.get(Calendar.DAY_OF_YEAR));
//这个月的最大天数
int k = c.getMaximum(Calendar.DAY_OF_MONTH);
//当前分钟
int h = c.get(Calendar.MINUTE);
//当前时间加10天
c.add(Calendar.DATE, 10);
//获取c的时间
System.out.println(c.get(Calendar.DATE));
```

* 计算一段时间的毫秒数

```java
//到所设定时间的毫秒数
Calendar c = Calendar.getInstance();	
c.set(1995, 03, 24);
long l1 = c.getTimeInMillis();
//到当前为止的毫秒数
Calendar c2 = Calendar.getInstance();
long l2 = c2.getTimeInMillis();
//所规定时间到当前的毫秒数
System.out.println(l2 - l1);
```

