### Random

```java
Random r = new Random();

System.out.println( r.nextInt() );

System.out.println( Math.abs(r.nextInt()%100) );

System.out.println( r.nextDouble() );

System.out.println( r.nextBoolean() );
```

```java
Random r = new Random();

byte [] b = new byte[10];

r.nextBytes(b);  //随机byte类型的数值，并存放于byte类型的数组中

System.out.println(Arrays.toString(b));

int n = r.nextInt(5)+15;

System.out.println("n-->" + n);
```

```java
Random r = new Random(5);

//r.setSeed(5);

System.out.println( r.nextInt(100) );
System.out.println( r.nextInt(100) );
System.out.println( r.nextInt(100) );

for (int i = 0; i < 10; i++) {
    System.out.println(r.nextInt(50));
}
```

