**session会话**

> 其本质上是一种接口
>
> session就是会话，是用来维护客户端和服务端之间关联的一种技术
>
> 每个客户端都会有一个自己的session会话
>
> session会话中，我们经常用来保存用户登录之后的信息
>
> 其存储的形式是以键值对的形式存储的

**创建和获取session判断是否是刚创建的session和获取id号**

> ```java
> //创建一个session
> HttpSession session = req.getSession();
> 
> //判断session是否是新创建的
> boolean aNew = session.isNew();
> 
> //可以获取每个session的特征id
> String id = session.getId();
> ```

**获取session里的数据**

> 首先获取到session然后再在其中有 setAttribute（）和 getAttribute（）
>
> ```java
> protected void setOrgetSession(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
> 
>     req.getSession().setAttribute("key1","value1");
> 
>     resp.getWriter().write("获取到了session");
> 
> }
> ```

**session的超时控制**

> 在session中可以类似与cookie的存活时间设置，同样可以进行存活时间的设置
>
> 设置方法为getMaxInactiveInterval()
>
> ```java
> //存活时间的设置
> session.setMaxInactiveInterval()
> //存活时间的获取
> session.getMaxInactiveInterval()     
> ```
>
> 失效时间的设置上默认为30分钟
>
> >如果想对整个项目的session进行设置可以在web.xml文件中进行设置
> >
> >```xml
> ><session-config>
> >    <session-timeout>20</session-timeout>>
> ></session-config>
> >```
>
> >如果想对单个的session进行设置，直接使用setMaxInactiveInterval()即可。
>
> **注意：**
>
> > 在session中的读秒时间是按照，一秒一秒的减的，只要是还在读秒的时间内创建session其session就是一直存在的
>
> invalidate（）函数可以让session马上失效，将session失效时间马上设置为0

session的底层实现原理

> **session其底层其实是基于cookie的。**
>
> **识别上主要是依靠发过来的cookie中id进行识别**
>
> ![image-20201223165853864](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20201223165853864.png)