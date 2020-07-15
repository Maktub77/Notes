#### 读文件并发上不去问题 org.apache.commons.io.FileUtils.readFileToByteArray()

apache的技术毋庸置疑，但使用时如果不加注意，坑还是有的。
**org.apache.commons.io.FileUtils.readFileToByteArray()** 方法能将文件转换为byte数组，让我们看一下它的源码

```java
/**
 * Reads the contents of a file into a byte array.
 * The file is always closed.
 *
 * @param file  the file to read, must not be {@code null}
 * @return the file contents, never {@code null}
 * @throws IOException in case of an I/O error
 * @since 1.1
 */
  public static byte[] readFileToByteArray(File file) throws IOException {
    InputStream in = null;
    try {
        in = openInputStream(file);
        return IOUtils.toByteArray(in, file.length());
    } finally {
        IOUtils.closeQuietly(in);
    }
}
```

可以看到此方法先将文件转换成InputStream **in = openInputStream(file);**
这个方法只是进行了简单的校验和检查，我们不必深究，下边returen尾调用中的方法
**IOUtils.toByteArray(in, file.length())** 是重点，可以看到参数包含一个文件流和一个long型的文件长度，然后我们看一下这个方法的源码及注释

```java
 /**
     * Get contents of an <code>InputStream</code> as a <code>byte[]</code>.
     * Use this method instead of <code>toByteArray(InputStream)</code>
     * when <code>InputStream</code> size is known.
     * <b>NOTE:</b> the method checks that the length can safely be cast to an int without truncation
     * before using {@link IOUtils#toByteArray(java.io.InputStream, int)} to read into the byte array.
     * (Arrays can have no more than Integer.MAX_VALUE entries anyway)
     * 
     * @param input the <code>InputStream</code> to read from
     * @param size the size of <code>InputStream</code>
     * @return the requested byte array
     * @throws IOException if an I/O error occurs or <code>InputStream</code> size differ from parameter size
     * @throws IllegalArgumentException if size is less than zero or size is greater than Integer.MAX_VALUE
     * @see IOUtils#toByteArray(java.io.InputStream, int)
     * @since 2.1
     */
public static byte[] toByteArray(InputStream input, long size) throws IOException {

  if(size > Integer.MAX_VALUE) {
      throw new IllegalArgumentException("Size cannot be greater than Integer max value: " + size);
  }

  return toByteArray(input, (int) size);
}
```

问题就出现在这里，首先它是static修饰的方法，作为一个程序员static可谓是见久不怪了，static修饰符修的方法在程序初始化时就会被加载，也是唯一一次加载，使得当前方法在内存中只有一个实例，极大地节约内存开销，但是在高并发的情况下，这个方法久可能会造成一个线程的数据还没读完，就被另一个线程覆盖了。所以这个方法在并发要求严格的项目中不适用。加锁更会导致项目在这个方法中运行缓慢。拒绝使用时最佳的解决方案。

## 然后说一说这个方法。

可能我们以为传入的是long类型，处理算法就真的是处理long类型才装的下的文件大小的大文件的方法，然而很搞笑，这个方法先把传入的 **file.length()** 检查一下，是否超过了四个字节的 Integer 型变量，超过了报异常，如果在Integer范围内，则尾调用 **toByteArray(input, (int) size);** 也就是真正将fileInoutStream 转换为byte数组的方法，这个方法没什么高深的地方，进行一系列的检查，然后调用java底层的 **read()** 方法，和我们直接调用 **file.read();** 类似。

同事被并发问题搞了好久，我帮忙解决一下，发现一个坑，特此发文帮广大码友避坑。

## 本人的解决方法

这个方法恰好是独立写的，不似面向对象编程，像极了函数式编程。我们可以把这个方法独立取出来放到我们自己的代码中

```java
public  byte[] readFileToByteArray(File file) throws IOException {
        InputStream in = null;
        try {
            in = openInputStream(file);
            return toByteArray(in, file.length());
        } finally {
            IOUtils.closeQuietly(in);
        }
    }
```

然后添加其调用的方法

```java
 public  FileInputStream openInputStream(File file) throws IOException {
        if (file.exists()) {
            if (file.isDirectory()) {
                throw new IOException("File '" + file + "' exists but is a directory");
            }
            if (file.canRead() == false) {
                throw new IOException("File '" + file + "' cannot be read");
            }
        } else {
            throw new FileNotFoundException("File '" + file + "' does not exist");
        }
        return new FileInputStream(file);
    }

    public  byte[] toByteArray(InputStream input, long size) throws IOException {

        if(size > Integer.MAX_VALUE) {
            throw new IllegalArgumentException("Size cannot be greater than Integer max value: " + size);
        }

        return toByteArray(input, (int) size);
    }

    public  byte[] toByteArray(InputStream input, int size) throws IOException {

        if (size < 0) {
            throw new IllegalArgumentException("Size must be equal or greater than zero: " + size);
        }

        if (size == 0) {
            return new byte[0];
        }

        byte[] data = new byte[size];
        int offset = 0;
        int readed;

        while (offset < size && (readed = input.read(data, offset, size - offset)) != EOF) {
            offset += readed;
        }

        if (offset != size) {
            throw new IOException("Unexpected readed size. current: " + offset + ", excepted: " + size);
        }

        return data;
    }
```

**方法是改好了，读文件不再加锁，但要小心服务器扛不住过多的内存占用，最好在方法调用之初的接口中就用线程池等方法限制请求数，保证不会因内存溢出而导致服务器宕机。**