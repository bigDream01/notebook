作用：

> 可以在页面跳转的时候可以不会路径丢失

**主页**

```html
<body>
        <!--
            这里的action表示的是点击会跳转的地址

            这个访问地址要与servlet的程序访问地址要对应 （就是在web.xml文件中对于servlet程序的地址配置）
        -->
    <form action="http://localhost:8080/Servlethello/hello1" method="get">
        <input type="submit">
    </form>
</body>
```

**分页**

```html
<head>
    <meta charset="UTF-8">
    <title>Title</title>
<!--    这里主要是写该位置的地址 z这样在跳转的时候不会出现跳转失败-->
    <base href="http://localhost:8080/Servlethello/a/b/c.html">
</head>
<body>
<a href="../../index.html">跳回首页</a><br/>

</body>
```

**servlet程序**

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    System.out.println("经过了servlet");
        //映射到web目录下
        request.getRequestDispatcher("/a/b/c.html").forward(request,response);
}
```

![image-20201109150739515](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20201109150739515.png)

**原理**