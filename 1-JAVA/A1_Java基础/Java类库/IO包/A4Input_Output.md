### InputStream字节输入流（抽象类）

![](C:\Users\Maktub_J\Documents\Notes\0-picture\A0_Photo\IO流\03-InputStream.png)

* 字节输入类的父类

* **AudioInputStream（高级流）**：音频数据的字节流

* ByteArrayInputStream：从字节数组中按字节读取数据

* FileInputStream：从文件中按字节读取数据

* FilterInputStream：

  * **BufferedInputStream（高级流）**：当获取到低级字节输入流可以转换为高级数据流
  * **DataInputStream（高级流）**：用来读取不同数据类型的数据（int/double......）

* **ObjectInputStream（高级流）**：用来读取Java中的对象（序列化反序列化）

* StringBufferInputStream：从字符串中按字节来读取数据

* 高级流是由低级流封装而来：

  ```java
  FileInputStream fis = new FileInputStream("D:/code/a.txt");
  //fis只能读取普通字节：
  BufferedInputStream bis = BufferedInputStream(fis);
  DataInputStream dis = new DataInputStream(fis);
  ObjectInputStream ois = new ObjectInputStream(fis);
  AudioInputStream ais = new AudioInputStream(fis);
  ```


#### 传输过程

```java
try {
    //1.创建一个文件字节输入流，指向要读取的源文件
    FileInputStream fis = new FileInputStream(new File("D:/壁纸/d.gif"));

    //2.获取文件中的字节个数时
    int  m = fis.available();
    System.out.println(m);

    //3.通过文件输入流，调用read()方法读取数据
    byte [] b = new byte[1024];
    int len = fis.read(b);//读取一个放到数组中去，得到数组的长度
    while(len!= -1){
        System.out.write(b, 0, len);
        len = fis.read(b);
    }

    //4.关闭流
    fis.close();

} catch (FileNotFoundException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
} catch (IOException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
}
```

### OutputStream字节输出流（抽象类）

![](C:\Users\Maktub\Documents\Notes\0-picture\A0_Photo\IO流\04-OutputStream.png)

- 字节输出流的父类

- ByteArrayOutputStream：将程序中的数据以字节形式写出到字节数组中

- FileOutputStream：将程序中的数据以字节形式写出到文件中

- FilterOutputStream :

  - **BufferedOutputStream（高级流）**：给低级缓冲区
  - **DataOutputStream（高级流）**：把带有数据类型的数据写到目标中
  - **PrintStream（高级流）**：扩展了低级流的功能

- **ObjectOutputStream（高级流）**：可以将Java中的对象保存到目标中

- 一般高级流都是由低级流封装而来：

  ```java
  FileOutputStream fos = new FileOutputStream("G:/bbb.txt");
  ```

- 低级字节流fos,只能将数据以字节的形式写出到文件中，所以我们需要写出一些特殊的数据时，需要将低级流扎转换为高级流；

  ```java
  BufferedOutputStream bof = new BuffredOutStream(fos);
  DataOutputStream dos = new DataOutputStream(fos);
  PrintStream ps = new PrintStream(fos);
  ObjectOutputStream oos = new ObjectOutputStream(fos);
  ```

- 通过文件输出流输出到外界的一个文件中，如果该文件不存在，则会自动创建文件

  ```java
  try {
      //FileOutputStream : 是字节输出流，可以将程序中的数据以字节形式输出到文件中
      //通过文件输出流输出到外界的一个文件中，如果该文件不存在，则会自动创建文件
      FileOutputStream fos = new FileOutputStream("D:/code/bb.txt");
  
      //一次写出一个字节
      //fos.write(97);
  
      byte [] b = new byte[1024];	
      for (int i = 0; i < b.length; i++) {
          b [i] = (byte)(97 + i);
      }
  
      //将参数指定的字节数组中的数据一次性写出到文件中
      //fos.write(b);
  
      //将第一个参数指定的字节数组中的数据，从第二个参数指定的索引开始，写第三个参数指定的字节个数
      fos.write(b, 0, 3);
  
      fos.close();
  
  } catch (FileNotFoundException e) {
      // TODO Auto-generated catch block
      e.printStackTrace();
  } catch (IOException e) {
      // TODO Auto-generated catch block
      e.printStackTrace();
  }
  ```

### FileCopy

```java
public static void main(String[] args) {
    copyFile("D:/code/d.gif","D:/code/d.png");
}

public static void copyFile(String src, String dest){
    FileInputStream fis = null;//定义全局变量
    FileOutputStream fos = null;

    try {
        fis = new FileInputStream( src );//源文件
        fos = new FileOutputStream( dest );//目标文件
//开始从上往下，先读后写
        byte [] b = new byte[1024];
        while(  fis.read(b) != -1){ //读			
            fos.write(b);	//写
        }

    } catch (FileNotFoundException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
    } catch (IOException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
    }finally{
//结束从下往上        
        try {
            if(fos != null){		
                fos.close();
            }
            if(fis != null ){
                fis.close();
            }
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        System.out.println("复制完成！");
    }
}

```

