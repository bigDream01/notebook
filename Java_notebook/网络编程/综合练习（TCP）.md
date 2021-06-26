客户端

```java
*@description:.客户端发送内容给服务端，服务端将内容打印到控制台上
 *@author：bigDream
 *@date：2020-09-24 15:38
 **/
public class Client {//客户端

    public static void main(String[] args) {

        Socket socket = null;//端口号
        OutputStream os = null;
        BufferedInputStream bis = null;
        InputStream is = null;
        try {

            //1.创建一个Socket端口对象
            InetAddress Inet = InetAddress.getByName("127.0.0.1");//ip号
            socket = new Socket(Inet, 8899);//端口号

            //2.创建socket输出流
            //相对于程序来说，是进行输出的内容，所以是outputStream。因为是socket所以调用getOutputStream（）
            os = socket.getOutputStream();

            //3.调用socket对象的方法尽行内容的输入
            bis = new BufferedInputStream(new FileInputStream(new File("20_9_24\\touxiang.JPG")));

            int len;
            byte[] buffer = new byte[1024];

            while ((len = bis.read(buffer)) != -1) {
                os.write(buffer, 0, len);
            }

            socket.shutdownOutput();
            is = socket.getInputStream();

            ByteArrayOutputStream baos = new ByteArrayOutputStream();

            //测试是否进入
            System.out.println("测试是否进入");
            int len1;
            byte[] buffer1 = new byte[5];

            while ((len1 = is.read(buffer1)) != -1) {

                baos.write(buffer1,0,len1);
            }
            System.out.println(baos.toString());


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

            if (bis != null) {

                try {
                    bis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (is != null) {
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }


    }

}
```

服务端

```java
*@description:.客户端发送内容给服务端，服务端将内容打印到控制台上
 *@author：bigDream
 *@date：2020-09-24 15:38
 **/
public class Service {//服务端

    public static void main(String[] args) throws IOException {

        ServerSocket ss = null;
        Socket socket = null;
        InputStream is = null;
        OutputStream os = null;


        try {

            //1.创建ServerSocket类的对象，并指明端口对象
            ss = new ServerSocket(8899);//接收Client传输的数据对象

            //2.调用accpet（）方法
            socket = ss.accept();//开启接收窗口

            //3.选择一个输入流，并进行传输
            //对于服务端来说此时的数据为输入的，所以应该是getInputStream（）
            is = socket.getInputStream();

            //主要是防止传输的时候出现内存溢出，导致传输的数据不完善
            // (其内部会自己造个数组用于存储数据，如果数据超过其开始的长度，那么其会自己扩容)

            os = new FileOutputStream(new File("20_9_24\\touxiang1.JPG"));


            byte[] buffer = new byte[1024];
            int len;

            while ((len = is.read(buffer)) != -1) {
                os.write(buffer, 0, len);
            }

            //反馈
            os = socket.getOutputStream();
            os.write("图片已收到，很好看！".getBytes());


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
            if (is != null) {
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (ss != null) {

                try {
                    ss.close();
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