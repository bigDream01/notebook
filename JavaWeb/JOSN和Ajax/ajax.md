**作用:**

> 主要是解决浏览器发起异步请求的时候解决方案，一种局部更新页面的技术

**底层原理**

> ![image-20210302140214505](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210302140214505.png)

**原生ajax请求的操作**

> 接收请求程序
>
> ```java
> protected void javaScript(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
> 
>     //解决中文乱码问题
>     req.setCharacterEncoding("UTF-8");
>     resp.setContentType("Test/html;charset=UTF-8");
> 
>     person p = new person(1,"张三");
>     Gson gson = new Gson();
>     String s = gson.toJson(p);
>     resp.getWriter().write(s);
> }
> ```
>
> JavaScript程序
>
> ```javascript
>     //发起服务器ajax请求
>          function ajaxRequest() {
> //              1、我们首先要创建XMLHttpRequest
>             var xhr = new XMLHttpRequest();
> //              2、调用open方法设置请求参数
>             xhr.open("GET","http://localhost:8080/JSON/ajaxServlet?action=javaScript",true);
> //           4、在send方法前绑定onreadystatechange事件，处理请求完成后的操作。
>             xhr.onreadystatechange = function () {
>                if(xhr.readyState == 4 && xhr.status == 200){
>                   document.getElementById("div01").innerHTML = xhr.responseText;
>                }
> 
>             }
> 
> //              3、调用send方法发送请求
>             xhr.send();
> //
>          }
> ```

**调用jQuery框架使用ajax**

> ```javascript
> $.ajax({
>    url:"http://localhost:8080/JSON/ajaxServlet",//请求地址
>    data:"action=jQueryAjax",//请求参数
>    type:"GET",//请求的方式
>     //请求成功后的回调函数
>     //在函数中的参数，就是数据
>    success:function (data) {
>       $("#msg").html("编号：" + data.id + "，姓名:" + data.name);
>    },
>     //请求返回的数据类型
>    dataType:"json"
> 
> })
> ```
>
> 如果datatype是text文件类型，就需要将text文件类型先转化为json对象、
>
> **使用get方法使用ajax（post类似就没做实例了）**
>
> ```JavaScript
> // ajax--get请求
> $("#getBtn").click(function () {
> 
>     $.get("http://localhost:8080/JSON/ajaxServlet", "action=jQueryGet", function (data) {
>         $("#msg").html("编号：" + data.id + "，姓名:" + data.name);
>     },"json");
> });
> ```
>
> > 在这个里就是将请求方式封装到方法名上，之后跟原装的一样。
> >
> > 不同的点在于其没有呈现键值对的方式。
> >
> > 其顺序分别为：
> >
> > ​     请求的地址
> >
> > ​     请求的参数
> >
> > ​     请求成功的回调函数
> >
> > ​     参数的类型
>
> **getjson方法**
>
> > 其就是封装了请求方式和参数类型，操作与get类似

**serialize（）方法的调用**

> **作用**
>
> >  可以获取到表单中的内容，并且将他们进行name=value&name=value拼接 
>
> **使用案例**
>
> > ```javascript
> > $("#submit").click(function () {
> >     // 把参数序列化
> >     // var json = $("#form01").serialize();
> >     $.getJSON("http://localhost:8080/JSON/ajaxServlet", "action=jQuerySerialize&" + $("#form01").serialize(), function (data) {
> >         $("#msg").html("编号：" + data.id + "，姓名:" + data.name);
> >     })
> > 
> > });
> > ```
>
> **注意点：**
>
> > ①在调用函数可以得到其序列化后的数值，在返回给服务器上参数之前需要使用“ & ”来进行连接
> >
> > ②在服务器端通过对表单的name属性来获取表单中的数据

