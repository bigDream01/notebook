### 通信要素一：IP和端口号

**如何实例化 InetAddress：**

> **public static InetAddress getLocalHost()**
>
> **public static InetAddress getByName(String host)**

**InetAddress的方法：**

> **public String getHostAddress()****：**返回 IP 地址字符串（以文本表现形式）。 
>
> **public String getHostName()****：**获取此 IP 地址的主机名
>
> **public boolean isReachable(int timeout)****：**测试是否可以达到该地址

**IP地址：**

> 唯一的标识 Internet 上的计算机（通信实体）
>
> 本地回环地址(hostAddress)：127.0.0.1 主机名(hostName)：localhost
>
> IP地址分类方式1：**IPV4** 和 **IPV6**
>
> IPV4：4个字节组成，4个0\-255。大概42亿，30亿都在北美，亚洲4亿。2011年初已经用尽。以点分十进制表示，如192.168.0.1
>
> IPV6：128位（16个字节），写成8个无符号整数，每个整数用四个十六进制位表示，数之间用冒号（：）分开，如：3ffe:3201:1401:1280:c8ff:fe4d:db39:1984
>
> IP地址分类方式2：**公网地址(万维网使用)和私有地址(局域网使用)**。192.168.开头的就是私有址址，范围即为192.168.0.0--192.168.255.255，专门为组织机构内部使用
>
> **本地回路地址：** 127.0.0.1(localhost)
>
> > ```java
> > InetAddress inet2 = InetAddress.getLocalHost();
> > ```
>
> **域名：**
>
> ![image-20200923161237487](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20200923161237487.png)

**端口号：**

> ① **端口号**标识正在计算机上运行的进程（程序）
>
> ② 不同的进程有不同的端口号
>
> ③ 被规定为一个 16 位的整数 0~65535 
>
> **端口分类：**
>
> >  **公认端口：**0~1023。被预先定义的服务通信占用（如：HTTP占用端口80，FTP占用端口21，Telnet占用端口23） 
> >
> >  **注册端口：**1024~49151。分配给用户进程或应用程序。（如：Tomcat占用端口8080，MySQL占用端口3306，Oracle占用端口1521等）。 
> >
> >  **动态/私有端口：**49152~65535。 

