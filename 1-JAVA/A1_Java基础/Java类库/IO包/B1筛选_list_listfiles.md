### list

* 返回String类型的数组

* FilenameFilter接口

  ```java
  File f = new File("D:/code");
  //创建匿名内部类：测试指定文件是否应该包含在某一文件列表中；
         Filter filter = new FilenameFilter() {
      @Override
      public boolean accept(File dir, String name) {
          if( name.endsWith(".txt")){ 
              return true;
          }else{
              return false;
          }	
      }
  };
  
  String [] arr2 = f.list(filter);
  for( String s : arr2){
      System.out.println(s);
  }
  ```


### listFiles

* 返回File类型的数组

  ```java
  File f = new File("D:/code");
  File [] file = f.listFiles();
  
  //创建匿名内部类
  FileFilter filter = new FileFilter() {
      @Override
      public boolean accept(File pathname) {
          if(pathname.getName().endsWith(".txt")){
              return true;
          }else{
              return false;
          }
      }
  }; 
  //打印处此路径下满足条件的文件的名称、大小以及最后修改时间
  File [] file2  = f.listFiles(filter);
  for(File ff : file2 ){                                           
      String name = ff.getName();
      long len = ff.length();
      long t = ff.lastModified();
      System.out.println(name + "\t" + len + "\t" + new Date(t).toLocaleString());
  }
  ```

### 遍历

* 遍历一个文件夹下所有子文件夹，打印文件的名称、大小和最后修改时间

  ```java
  //创建一个含File参数的printFileInfo方法
  public void printFileInfo(File f){
      if( f.isFile() ){	//判断是否代表一个文件
          String name = f.getName(); 	//获取文件名
          long len = f.length();		//获取文件大小
          long t = f.lastModified();	//获取文件最后修改时间
          Date d = new Date(t);	
          System.out.println("--" + name + "\t" + len/1024.0/1024.0 + "\t" + d.toLocaleString());
      }else{
          //打印出目录（文件夹）名称
          System.out.println("【目录名称】" + f.getName());
          File [] cfs = f.listFiles();
          for( File cf : cfs){
              //递归，再次调用上述方法
              printFileInfo(cf);
          }
      }
  }
  ```

* 创建主函数调用方法

  ```java
  public static void main(String[] args) {
      File f = new File("D:/code");
      new Test03().printFileInfo(f);
  }
  ```

  