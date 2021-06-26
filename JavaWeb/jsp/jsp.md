### jsp的本质

> jsp的本质就是一个servlet程序
>
> 当我们第一次访问的页面的时候。tomcat会把jsp文件翻译成java源文件，并进行编译为.class的字节码文件

 

### jsp 的三种语法
**a)jsp 头部的page 指令**(**少用不要动**) 
			jsp 的page 指令可以修改jsp 页面中一些重要的属性，或者行为。

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>

```

​		i. language 属性表示jsp 翻译后是什么语言文件。暂时只支持java。

​		ii. contentType 属性表示jsp 返回的数据类型是什么。也是源码中response.setContentType()参数值

​		iii. pageEncoding 属性表示当前jsp 页面文件本身的字符集。

​		iv. import 属性跟java 源代码中一样。用于导包，导类。

========================两个属性是给out 输出流使用=============================

​		v. autoFlush 属性设置当out 输出流缓冲区满了之后，是否自动刷新冲级区。默认值是true。

​		   vi. buffer 属性设置out 缓冲区的大小。默认是8kb

​		viii. isErrorPage 属性设置当前jsp 页面是否是错误信息页面。默认是false。如果是true 可以获取异常信息。

​			ix. session 属性设置访问当前jsp 页面，是否会创建HttpSession 对象。默认是true。

​			x. extends 属性设置jsp 翻译出来的java 类默认继承谁。

**ii. 表达式脚本**（**常用**）
			表达式脚本的格式是：<%=表达式%>
			表达式脚本的作用是：的jsp 页面上输出数据。
			表达式脚本的特点：
				1、所有的表达式脚本都会被翻译到_jspService() 方法中
				2、表达式脚本都会被翻译成为out.print()输出到页面上
				3、由于表达式脚本翻译的内容都在_jspService() 方法中,所以_jspService()方法中的对象都可以直接使用。
				4、表达式脚本中的表达式不能以分号结束。

![image-20201120215009156](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20201120215009156.png)



### jsp 九大内置对象

![image-20201120215225338](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20201120215225338.png)

### jsp 四大域对象

**四个域对象分别是：**

> pageContext 		(PageContextImpl 类) 		当前jsp 页面范围内有效
> 		request 		(HttpServletRequest 类)、	一次请求内有效
> 		session 		(HttpSession 类)、		一个会话范围内有效（打开浏览器访问服务器，直到关闭浏览器）
> 		application 		(ServletContext 类)		 整个web 工程范围内都有效（只要web 工程不停止，数据都在）

域对象是可以像Map 一样存取数据的对象。四个域对象功能一样。不同的是它们对数据的存取范围。
虽然四个域对象都可以存取数据。在使用上它们是有优先顺序的。

**四个域在使用的时候，优先顺序分别是，他们从小到大的范围的顺序。**

> pageContext ====>>> request ====>>> session ====>>> application 

```jsp
<body>

<h1>scope.jsp 页面</h1>

<%
// 往四个域中都分别保存了数据
        pageContext.setAttribute("key", "pageContext");
        request.setAttribute("key", "request");
        session.setAttribute("key", "session");
        application.setAttribute("key", "application");
        %>
        pageContext 域是否有值：<%=pageContext.getAttribute("key")%> <br>
        request 域是否有值：<%=request.getAttribute("key")%> <br>
        session 域是否有值：<%=session.getAttribute("key")%> <br>
        application 域是否有值：<%=application.getAttribute("key")%> <br>
        <%
        request.getRequestDispatcher("/scope2.jsp").forward(request,response);
%>
</body>
```

### jsp 的输出
> 由于jsp 翻译之后，底层源代码都是使用out 来进行输出，所以一般情况下。我们在jsp 页面中统一使用out 来进行输出。避免打乱页面输出内容的顺序。

​			out.write() 输出字符串没有问题

​		   out.print() 输出任意数据都没有问题（都转换成为字符串后调用的write 输出）

深入源码，浅出结论：在jsp 页面中，可以统一使用out.print()来进行输出

### jsp的标签

**静态包含（常用）**

> ```jsp
> <%@include file="/include/b.jsp"%>
> ```
>
> file		表示传输的文件
>
> / 表示当前工程下目录 “ 填写文件路径 ”

**特点**：

> 主要是对传输的文件拷贝到该位置然后输出

**静态包含**

> ```jsp
> <jsp:include page="b.jsp"></jsp:include>
> ```
>
> 因为不常用就不做介绍

**请求转发**

> ```jsp 
> <%--请求转发/--%>
>         <jsp:forward page="b.jsp"></jsp:forward>
> ```

listener监听器

