### Sender

```java
public class Sender {
    public static void main(String[] args) {
        try {
            //1.准备要发送的数据(从控制台输入)
            System.out.println("请输入要发送的数据：");
            Scanner sc = new Scanner(System.in);
            String msg = sc.nextLine();

            //2.创建数据报/包
            //①如果DatagramPacket对象是用来发送数据的，则需要知道发送目标的IP和port
            //②实例化DatagramPacket对象时，第一个参数：发送的数据 b
            //						    第二个参数：发送数据的长度 b.length
            //						    第三个参数：发送的目标地址 addr
            //						    第四个参数：端口号 9696
            byte [] b = msg.getBytes();
            InetAddress addr = InetAddress.getByName("127.0.0.1");
            DatagramPacket dp = new DatagramPacket(b, b.length , addr, 9696);

            //3.创建DatagramSocket用来发送数据包
            //使用send()方法发送数据包
            DatagramSocket ds = new DatagramSocket();
            ds.send(dp);

            System.out.println("信息发送完成！");

        } catch (UnknownHostException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (SocketException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

### Receiver

```java
public class Receiver {
    public static void main(String[] args) {
        try {
            System.out.println("接收端程序");
            //1.创建DatagramSocket对象，用于接收数据包
            DatagramSocket ds = new DatagramSocket(9696);

            //2.创建DatagramPacket对象，用于接收数据
            //②如果创建的DatagramPacket是用来接收数据的，无需指定地址
            byte [] bs = new byte[1024];
            DatagramPacket dp = new DatagramPacket(bs, bs.length);
            ds.receive(dp);

            //3.从数据包中取出数据
            bs = dp.getData();

            //数据包中接收到数据的  字节数
            int len = dp.getLength();	
            System.out.write(bs ,0 , len);
            System.out.println(len);

        } catch (SocketException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

![](C:\Users\Maktub\Documents\Notes\0-picture\A0_Photo\网络编程\DatagramSocket.png)