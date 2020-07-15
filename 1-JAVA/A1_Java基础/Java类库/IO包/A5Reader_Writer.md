### Reader（字符输入流）

* ```java
  FileReader fr = null;
  
  try {
      fr = new FileReader("D:/code/dd.txt");
  
      int k;
      while((k = fr.read()) != -1){
          System.out.print((char)k);
      }
  
  } catch (FileNotFoundException e) {
      // TODO Auto-generated catch block
      e.printStackTrace();
  } catch (IOException e) {
      // TODO Auto-generated catch block
      e.printStackTrace();
  }finally{
      try {
          if(fr != null){
              fr.close();
          }	
      } catch (IOException e) {
          // TODO Auto-generated catch block
          e.printStackTrace();
      }
  }
  ```

### Writer（字符输出流）

* ```java
  FileWriter fw = null;
  
  try {
      fw = new FileWriter("D:/code/aa.txt");
  
      fw.write("100");
  
      for (int i = 0; i < 1000; i++) {		
          if( i % 50 == 0){
              fw.write("\n"); 	//输出换行
          }
          fw.write(i);
      }
  
  } catch (IOException e) {
      // TODO Auto-generated catch block
      e.printStackTrace();
  }finally{
      if(fw != null){
          try {
              fw.close();
          } catch (IOException e) {
              // TODO Auto-generated catch block
              e.printStackTrace();
          }
      }
  }
  ```





