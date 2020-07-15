### MulticastSocket

```java
public class MultiSender {
    public static void main(String[] args) {
        try {
            //1.准备数据
            Scanner sc = new Scanner(System.in);
            System.out.println("请输入要发送的信息：");
            String msg = sc.nextLine();

            //2.创建数据包/报
            byte [] bs = msg.getBytes();
            InetAddress addr = InetAddress.getByName("228.5.6.7");
            DatagramPacket p = new DatagramPacket(bs, bs.length, addr , 8989);

            //3.通过MulticastSocket（子类）地址簇，发送数据
            MulticastSocket ms = new MulticastSocket();
            ms.joinGroup(addr);
            ms.send(p);

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

### Receiver

```java
public class Receiver {
    public static void main(String[] args) {
        try {
            System.out.println("-----------receiver01");
            MulticastSocket ss = new MulticastSocket(8989);

            InetAddress addr = InetAddress.getByName("228.5.6.7");

            ss.joinGroup(addr);

            byte [] bs = new byte[1024];
            DatagramPacket p = new DatagramPacket(bs, bs.length);
            ss.receive(p);

            bs = p.getData();

            int len = p.getLength();

            System.out.write(bs, 0, len);

        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```



