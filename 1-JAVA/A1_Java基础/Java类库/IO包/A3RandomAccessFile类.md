### RandomAccessFile

* read

  * ```java
    //从raf中读出东西放在b数组中
    raf.read(b);
    //从raf中读出东西存放到b数组中，从第十个位置开始存放，共50个元素
    raf.read(b, 10, 50);
    //读取一行
    raf.readLine()
    ```

* write

  * ```java
    raf.writeInt(10);
    raf.seek(file.length());  //从文件末尾开始写入
    raf.writeChars("Hello World!!!"); //写入Hello World!!!
    ```


#### 用户注册

```java
public static void main(String[] args) throws IOException {

    Scanner sc = new Scanner(System.in);
    System.out.println("请输入用户名：");
    String name = sc.next();
    
    //判断输入的用户名是否与正则表达式匹配
    if(!name.matches(("^[a-zA-Z]+$"))){
        System.out.println("你输入的用户名不符合规则，请重新输入！！！");
        name = sc.next();
        if(!name.matches(("^[a-zA-Z]+$"))){
            System.out.println("不符合规则，注册失败，退出程序！");
            System.exit(0);
        }
    }
    System.out.println("请输入密码：");
    String pwd = sc.next();

    RandomAccessFile raf = new RandomAccessFile("D:/code/aa.txt", "rw");//可读可写

    raf.writeBytes(name);//写入用户名
    raf.writeBytes("#");
    raf.writeBytes(pwd);//写入密码

    raf.close();//关闭
    System.out.println("注册成功!!!");	
	}
}
```

#### 用户登录

```java
public static void main(String[] args) throws IOException {
    Scanner sc = new Scanner(System.in);
    System.out.println("请输入用户名：");
    String name = sc.next();
    System.out.println("请输入密码：");
    String pwd = sc.next();

    RandomAccessFile raf = new RandomAccessFile("D:/code/aa.txt","r");//只读

    String str = raf.readLine();//读取一行

    String [] arr =str.split("#");//以#为界拆分为String类型的数组		

    if( name.equals(arr[0]) && pwd.equals(arr[1])){
        System.out.println("登录成功！");
    }else{
        System.out.println("登录失败！");
    }
}
```

