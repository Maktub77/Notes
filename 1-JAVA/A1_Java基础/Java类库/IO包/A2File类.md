### File类

* 通过File类的构造器创建一个File对象：

  ```java
  File f = new File("D:/code/dd.txt");
  //File f = new file("D:\\code\\dd.txt");
  //创建一个新的文件
  f.createNewFile();
  //创建一个新的文件夹
  f.mkdir();
  ```

  1. 如果参数路径有后缀名则此File对象表示一个文件
  2. 如果参数路径没有后缀名，则File对象表示一个文件夹/目录

* File类中提供关于文件/文件夹的一些操作方法

* File类提供的关于获取文件相关信息的方法

  1. 获取文件的读写状态

     ```java
     boolean flag1 = f.canRead();	//可读
     boolean flag2 = f.canWrite();   //可写
     ```

  2. 判断是否存在

     ```java
     //判断文件/文件夹是否存在
     boolean flag3 = f.exists();
     //判断File对象是否代表一个文件
     boolean flag4 = f.isFile();
     //判断File对象是否代表一个目录
     boolean flag5 = f.isDirectory();
     ```

  3. 判断是否隐藏

     ```java
     //判断当前文件是否是一个隐藏文件
     boolean flag6 = f.isHidden();
     ```

  4. 获取文件/文件夹的名字、路径

     ```java
     String fileName = f.getName();
     String filePath = f.getPath();
     ```

  5. 获取父目录路径

     ```java
     String parentPath = f.getParent();	//String
     File parentfile = f.getParentFile();//File对象
     ```

  6. 是否删除

     ```java
     if( f.delete()){
         System.out.println("删除成功！");
     }else{
         System.out.println("删除失败！");
     }
     ```

  7. 长度(字符字节长度，一个汉字两个字符字节)

     ```java
     System.out.println( f.length());
     ```

  8. 文件最后一次修改的时间，一毫秒计

     ```java
     System.out.println( f.lastModified());
     Date d = new Date(f.lastModified());
     System.out.println(d.toLocaleString());
     ```

  9. list() : 列举出这个路径下的所有文件和文件夹的名字（包含隐藏文件）

     ```java
     //list() : 列举出这个路径下的所有文件和文件夹的名字（包含隐藏文件）
     File f = new File("E:/J1805");
     String [] arr = f.list();
     for(String s:arr){
         System.out.println(s);
     }
     //listFiles() : 列举出这个路径下的所有文件和文件夹的名字（包含隐藏文件）
     File [] file = f.listFiles();
     for(File ff : file){
         System.out.println( ff.getName());
     }
     ```

     