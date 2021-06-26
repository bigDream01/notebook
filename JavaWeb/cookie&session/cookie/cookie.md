**cookie的作用**

> cookie 服务器通知客户端保存键值对的一种技术
>
> 客户端有了cookie后，每次请求都发送给服务器
>
> 每个cookie的大小不能超过4KB
>
> 其保存在客户端

**创建cookie**

> 在servlet程序中创建一个cookie，然后在响应上添加addCookie

```java
Cookie cookie = new Cookie("key1","value1");
resp.addCookie(cookie);
```

**获取cookie**

> 请求域中通过getCookie( ) 得到cookie数组，然后使用循环得到指定的cookie

```java
Cookie[] cookies = req.getCookies();
Cookie cookie = CookieUtil.getCookie("key1", cookies);
```

```java
public static Cookie getCookie( String name,Cookie[] cookies){

    if(name == null || cookies.length == 0 || cookies == null) {
        return null;
    }

    for (Cookie cookie : cookies) {

        if(name.equals(cookie.getName())) {
            return cookie;
        }
    }
    return null;
}
```

**修改cookie**

> 在修改cookie上主要使用两种方式进行修改

```java
Cookie[] cookies = req.getCookies();
//方案一：创建一个cookie然后进行覆盖

    //1.在创建新cookie的时候要将cookie的key与原来的cookie的key要一致,然后改变value值
Cookie cookie = new Cookie("key1","value2");
    //2.使用add()方法添加cookie
resp.addCookie(cookie);
resp.getWriter().write("修改了cookie");

//方案二：获取需要修改的cookie然后在该cookie上进行修改
 //   1.先找到对应值的Cookie

Cookie cookie1 = CookieUtil.getCookie("key1", cookies);
//2.采用setValue（）方法进行设置value值
cookie1.setValue("value");
resp.getWriter().write("修改了cookie");
```

**cookie的存活控制**

> setMaxAge() 
>
> > 正值表示cookie存储的最大时间
> >
> > 负值表示浏览器一关，cookie值便关闭
> >
> > 0表示马上删除cookie
>
> 默认情况下cookie的设置的的MaxAge为-1，即关闭浏览器的时候是马上关闭cookie

**删除cookie**

```java
//先获取到所有的cookie
Cookie[] cookies = req.getCookies();
//先找到需要删除的cookie
Cookie cookie = CookieUtil.getCookie("key1", cookies);
if(cookie != null) {
    //然后将其的age设置为0
    cookie.setMaxAge(0);
    //将cookie添加到浏览器中
    resp.addCookie(cookie);
    //设置浏览器显示
    resp.getWriter().write("删除成功");
}
```

**path属性的设置**

> 可以通过对于path的设置从而达到过滤的目的
>
> > 其过滤的原理就是判断是否是包含了该路径，如果包含就发送，不包含就不发送
>
> cookie  A       http：//ip：port/项目路径
>
> cookie  B       http：//ip：port/项目路径/abc
>
> 请求的地址为：
>
> ​        http：//ip：port/项目路径    只发送A
>
> ​        http：//ip：port/项目路径/abc      发送A，B

```java
Cookie cookie = new Cookie("path1","path1");
cookie.setPath(req.getContextPath() + "/abc");
resp.addCookie(cookie);
resp.getWriter().write("创建了一个带有ABC的cookie");
```

