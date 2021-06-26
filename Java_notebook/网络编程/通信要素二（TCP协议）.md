**TCP:**

>  可靠的数据传输，可以进行大数据量的传输，效率低

**TCP演示**

```java
/*
 *
 *
 *
 *@description:.客户端发送内容给服务端，服务端将内容打印到控制台上
 *@author：bigDream
 *@date：2020-09-24 15:38
 **/
public class Client {
    public static void main(String[] args) {

        Socket socket = null;//端口号
        OutputStream os = null;
        try {

            //1.创建一个Socket端口对象
            InetAddress Inet = InetAddress.getByName("127.0.0.1");//ip号
            socket = new Socket(Inet, 8899);//端口号

            //2.创建socket输出流
            os = socket.getOutputStream();//相对于程序来说，是进行输出的内容，所以是outputStream。因为是socket所以调用getOutputStream（）

            //3.调用socket对象的方法尽行内容的输入
            os.write("你好".getBytes());//getBytes是对数据的byte数字化

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4.关闭流
            if (socket != null) {
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
            if (os != null) {

                try {
                    os.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }


    }

}
```

```java
*@description:.客户端发送内容给服务端，服务端将内容打印到控制台上
 *@author：bigDream
 *@date：2020-09-24 15:38
 **/
public class Service {
    public static void main(String[] args) throws IOException {

        ServerSocket ss = null;
        Socket socket = null;
        InputStream is = null;
        ByteArrayOutputStream baos = null;
        try {

            //1.创建ServerSocket类的对象，并指明端口对象
            ss = new ServerSocket(8899);//接收Client传输的数据对象

            //2.调用accpet（）方法
            socket = ss.accept();//开启接收窗口

            //3.选择一个输入流，并进行传输
            is = socket.getInputStream();//对于服务端来说此时的数据为输入的，所以应该是getInputStream（）

            baos = new ByteArrayOutputStream();//主要是防止传输的时候出现内存溢出，导致传输的数据不完善

            byte[] buffer = new byte[20];
            int len;

            while ((len = is.read(buffer)) != -1) {
                baos.write(buffer, 0, len);
                System.out.println(baos.toString());

            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
          //4.关闭流
            if (baos != null) {

                baos.close();
            }
            if (socket != null) {

                socket.close();
            }
            if (is != null) {
                is.close();
            }
            if (ss != null) {

                ss.close();
            }
        }


    }
}
```