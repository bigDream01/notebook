**第一个请求重定向的类**

```java
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    System.out.println("经过request1");
    //请求重定向
    resp.sendRedirect("http://localhost:8080/Servlethello/response2");
}
```

第二个请求重定向的类

```java
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    System.out.println("经过request2");
}
```

特点：

> ①浏览器的地址栏会发生变化
>
> ②两次请求
>
> ③不会共享request数据（因为是两次请求）
>
> ④不能访问 web—INF的文件
>
> ⑤可以访问服务器之外的地址