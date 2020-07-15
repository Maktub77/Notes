### FileDownloadServer

```java
public class FileDownloadServer {
    public static void main(String[] args) {
        try {
            ServerSocket server = new ServerSocket(7777);
            Socket socket = server.accept();

            InputStream is = socket.getInputStream();
            OutputStream os = socket.getOutputStream();

            InputStreamReader isr = new InputStreamReader(is);
            BufferedReader br = new BufferedReader(isr);			
            PrintWriter pw = new PrintWriter(os);

            //1.服务端向客户端发送连接提示
            pw.println("-------【文件下载器欢迎你】-------");
            pw.flush();

            //3.服务端向客户端发送信息，将可提供下载的文件列表发送给客户端
            File dir = new File("D:/code");
            File [] fs = dir.listFiles();
            for(File f : fs){
                if( f.isFile()){
                    pw.println( f.getName());
                    pw.flush();
                }
            }
            pw.println("File List End"); 
            pw.flush();

            //7.服务端接收客户端发送的要下载的文件名
            String filename = br.readLine();
            File f = new File(dir, filename);

            //8.服务端读取文件。通过流发送给客户端
            FileInputStream fis = new FileInputStream(f);
            byte [] bs = new byte[1024];
            int len = -1;
            while ( ( len = fis.read(bs)) != -1){
                os.write(bs, 0, len);
            }

            os.close();
            fis.close();
            socket.close();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

### FileDownloadClient

```java
public class FileDownloadClient {
    public static void main(String[] args) {
        try {
            Socket socket = new Socket("192.168.0.98", 7777);

            InputStream is = socket.getInputStream();
            OutputStream os = socket.getOutputStream();

            InputStreamReader isr = new InputStreamReader(is);
            BufferedReader br = new BufferedReader(isr);			
            PrintWriter pw = new PrintWriter(os);

            //2.客户端接收服务端的提示信息
            String tips = br.readLine();
            System.out.println(tips);

            //4.客户端接收服务端发送的可下载的文件列表（文件名称）
            while(true){
                String fn = br.readLine();
                if("File List End".equals(fn)){
                    break;
                }
                System.out.println(fn);
            }

            //5.客户端从控制台输入要下载的文件名称并发送给服务端
            Scanner sc = new Scanner(System.in);
            System.out.println("请输入要下载的文件名：");
            String fileName = sc.nextLine();
            pw.println(fileName);
            pw.flush();

            //6.客户端从控制台输入文件保存的目录
            System.out.println("请输入文件保存的目录：");
            String saveDir = sc.nextLine();

            //9.客户端通过is读取服务端发送的文件数据并保存到本地
            String path = saveDir +":/" + fileName;
            FileOutputStream fos = new FileOutputStream(path);
            byte [] bs = new byte[1024];
            int len = -1;
            while( (len =is.read(bs)) != -1){
                fos.write(bs, 0, len);
            }	
            os.close();
            is.close();
            fos.close();
            socket.close();
            System.out.println("下载完成！");

        } catch (UnknownHostException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```





