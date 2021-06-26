**注意点：**

>  servletConfig是由Tomcat服务器来进行创建的
>
>  servletConfig只能在自己servlet的程序使用
>
> ```java
> //在配置servletConfig对象的时候要继承父类的方法，其会保存servletConfig否则在后面会出现空指针异常
> super.init(config);
> ```

**servletConfig的作用**

```java
1//获取servlet的别名
String servletName = servletConfig.getServletName();
System.out.println("在init中被访问别名：" + servletName);

//获取初始化的init-param
String url = servletConfig.getInitParameter("url");
System.out.println(url);

//获取servletContext对象
ServletContext servletContext = servletConfig.getServletContext();
System.out.println("servletContext对象" + servletContext);
```

**如果获取InitParameter的值，参数是通过配置文件中的值来进行配对的**

```xml
<servlet>
        <!-- 给servlet程序起一个别名   (一般为程序名)-->
    <servlet-name>servletTest</servlet-name>
        <!-- 访问类名的地址 -->
    <servlet-class>Servlethello.servletTest</servlet-class>

    <init-param>
        <!--     参数名       -->
        <param-name>url</param-name>
        <!--     参数值        -->
        <param-value>jdbc:mysql//localhost:3306</param-value>
    </init-param>
</servlet>
```