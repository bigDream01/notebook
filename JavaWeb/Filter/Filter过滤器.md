**作用：**

> 其主要是实现权限管理，可以实现对于权限的过滤
>
> 过滤的主要是对设置区域内请求的检查，且不会对所有请求检查，只会对于访问filter限制范围内的区域进行权限检查
>
> ![image-20210109154601485](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210109154601485.png)

**实现步骤**

> ① 先创建一个实现filter接口的类（为这个接口javax.servlet.*）
>
> ②dofilter（）中进行过滤条件的创建
>
> > **登录检验filter**
> >
> > ```java
> > public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
> >     HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
> > 
> >     HttpSession session = httpServletRequest.getSession();
> > 
> >     Object user = session.getAttribute("user");
> >     if(user == null ) {
> >         httpServletRequest.getRequestDispatcher("/login.jsp").forward(servletRequest,servletResponse);
> >         return;
> >     }else {
> >         //让服务器访问下面的资源
> >         filterChain.doFilter(servletRequest,servletResponse);
> >     }
> > 
> > 
> > }
> > ```
>
> ③在web.xml中对filter实现类进行配置
>
> > ```xml
> >     <filter>
> >     <filter-name>AdminFilter</filter-name>
> >     <filter-class>com.atguigu.filter.AdminFilter</filter-class>
> > </filter>
> >     <filter-mapping>
> >         <filter-name>AdminFilter</filter-name>
> >         
> > <!--       在这里对限制区域进行配置-->
> >         <url-pattern>/admin/*</url-pattern>
> >     </filter-mapping>
> > ```

**filter的生命周期**

> ①构造器
>
> ②init（）
>
> > **在web工程启动的时候就直接执行**
>
> ③dofilter（）
>
> > 拦截到请求就会执行
>
> ④destroy（）
>
> > 停止web工程的时候就会销毁

**FilterConfig的使用**

> 这个类是存在于init（）方法中的
>
> ![image-20210109154238888](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210109154238888.png)
>
> 其主要是存在三种作用：
>
> > 一、获取filter在init-param初始化的信息
> >
> > 二、获取filter在filter-name的内容
> >
> > 三、获取servletContext对象

**FilterChain过滤链**

> ![image-20210109160827754](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210109160827754.png)

**路径匹配**

> 精确匹配
>
> > 就是直接精确到页面，对单独的页面进行匹配
>
> 目录匹配
>
> > ```xml
> > <filter-mapping>
> >         <filter-name>AdminFilter</filter-name>
> > 
> > <!--       在这里对限制区域进行配置-->
> >         <url-pattern>/admin/*</url-pattern>
> >     </filter-mapping>
> > ```
> >
> > 对于一个目录下的整个文件进行操作
>
> 后缀匹配
>
> > 对于整个项目中所有的该后缀的页面都会进行过滤（这里的后缀是所有的文字都可以的）
> >
> > ```xml
> >         <url-pattern>*.html</url-pattern>
> > ```
> >
> > 注意点：
> >
> > > 不能以“  / ”开始
>
> **filter过滤器只关心请求的地址是否匹配，不关心请求的资源是否存在**

**filter与ThreadLocal的组合使用**

> **作用**
>
> > 对程序中的出进行捕获并处理
>
> **threadlocal**
>
> >  主要是利用Threadlocal的特性，将每一个从连接池中获取到的连接直接装入到threadlocal中，然后结合jdbc中的事务对程序中的异常情况进行处理。
>
> **filter**
>
> > 主要是结合threadlocal在对每一次请求都进行过滤包装，这样就不需要对每一个servlet和service程序进行try-catch
>
> **注意点**
>
> > ①在进行filter过滤的时候一定要将底层的异常向上抛出，不然在filter中没有办法对异常进行捕获（因为底层上对异常已经处理了）
> >
> > ②在对异常处理完成后，要对用户编写提示页面
> >
> > ③在使用threadlocal后对其进行remove操作不然会造成内存溢出



