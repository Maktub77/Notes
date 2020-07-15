### URL

```JAVA
public static void main(String[] args) {
    try {
        URL u = new URL("http://192.168.0.254:80/sims/bbb/kb.jpg");

        //获取当前资源的网络连接
        URLConnection c = u.openConnection();

        InputStream is = c.getInputStream();

        FileOutputStream fos = new FileOutputStream("D:/code/kb.jpg");

        //通过is读取网络中的文件资源，通过fos保存到本地
        byte [] bs = new byte[1024];
        int len = -1;
        while( (len = is.read(bs)) != -1){
            fos.write(bs, 0, len);
        }
        fos.close();
        is.close();
        System.out.println("下载成功！");

    } catch (MalformedURLException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
    } catch (IOException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
    }
}
```

